---
title: "Managing JWT Security"
source: "https://docs.nats.io/running-a-nats-service/nats_admin/security.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/nats_admin/security"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Managing JWT Security"
---
# Managing JWT Security

If you are using the [JWT](/running-a-nats-service/configuration/securing_nats/auth_intro/jwt.md) model of authentication to secure your NATS infrastructure or implementing an [Auth callout](/running-a-nats-service/configuration/securing_nats/auth_callout.md) service, you can administer authentication and authorization without having to change the servers' configuration files.

You can use the [`nsc`](/using-nats/nats-tools/nsc.md) CLI tool to manage identities. Identities take the form of nkeys. Nkeys are a public-key signature system based on Ed25519 for the NATS ecosystem. The nkey identities are associated with NATS configuration in the form of a JSON Web Token (JWT). The JWT is digitally signed by the private key of an issuer forming a chain of trust. The nsc tool creates and manages these identities and allows you to deploy them to a JWT account server, which in turn makes the configurations available to nats-servers.

You can also use [`nk`](https://github.com/nats-io/nkeys#readme) CLI tool and library to manage keys.

## Creating, updating and managing JWTs programmatically

You can create, update and delete accounts and users programmatically using the following libraries:

* Golang: see [NKEYS](https://github.com/nats-io/nkeys) and [JWT](https://github.com/nats-io/jwt/tree/main/v2).
* Java: see [NKey.java](https://github.com/nats-io/nats.java/blob/main/src/main/java/io/nats/client/NKey.java) and [JwtUtils.java](https://github.com/nats-io/nats.java/blob/main/src/main/java/io/nats/client/support/JwtUtils.java)

## Integrating with your existing authentication/authorization system

You can integrate NATS with your existing authentication/authorization system or create your own custom authentication using the [Auth callout](/running-a-nats-service/configuration/securing_nats/auth_callout.md).

### Examples

See [NATS by Example](https://natsbyexample.com/) under "Authentication and Authorization" for JWT and Auth callout server implementation examples.

#### User JWTs

[User creation in Golang](/running-a-nats-service/nats_admin/security/jwt.md#automated-sign-up-services---jwt-and-nkey-libraries)

#### Account JWTs

Golang example from <https://natsbyexample.com/examples/auth/nkeys-jwts/go>

```
package main

import (
	"flag"
	"fmt"
	"log"

	"github.com/nats-io/jwt/v2"
	"github.com/nats-io/nkeys"
)

func main() {
	log.SetFlags(0)

	var (
		accountSeed  string
		operatorSeed string
		name         string
	)

	flag.StringVar(&operatorSeed, "operator", "", "Operator seed for creating an account.")
	flag.StringVar(&accountSeed, "account", "", "Account seed for creating a user.")
	flag.StringVar(&name, "name", "", "Account or user name to be created.")

	flag.Parse()

	if accountSeed != "" && operatorSeed != "" {
		log.Fatal("operator and account cannot both be provided")
	}

	var (
		jwt string
		err error
	)

	if operatorSeed != "" {
		jwt, err = createAccount(operatorSeed, name)
	} else if accountSeed != "" {
		jwt, err = createUser(accountSeed, name)
	} else {
		flag.PrintDefaults()
		return
	}
	if err != nil {
		log.Fatalf("error creating account JWT: %v", err)
	}

	fmt.Println(jwt)
}

func createAccount(operatorSeed, accountName string) (string, error) {
	akp, err := nkeys.CreateAccount()
	if err != nil {
		return "", fmt.Errorf("unable to create account using nkeys: %w", err)
	}

	apub, err := akp.PublicKey()
	if err != nil {
		return "", fmt.Errorf("unable to retrieve public key: %w", err)
	}

	ac := jwt.NewAccountClaims(apub)
	ac.Name = accountName

	// Load operator key pair
	okp, err := nkeys.FromSeed([]byte(operatorSeed))
	if err != nil {
		return "", fmt.Errorf("unable to create operator key pair from seed: %w", err)
	}

	// Sign the account claims and convert it into a JWT string
	ajwt, err := ac.Encode(okp)
	if err != nil {
		return "", fmt.Errorf("unable to sign the claims: %w", err)
	}

	return ajwt, nil
}

func createUser(accountSeed, userName string) (string, error) {
	ukp, err := nkeys.CreateUser()
	if err != nil {
		return "", fmt.Errorf("unable to create user using nkeys: %w", err)
	}

	upub, err := ukp.PublicKey()
	if err != nil {
		return "", fmt.Errorf("unable to retrieve public key: %w", err)
	}

	uc := jwt.NewUserClaims(upub)
	uc.Name = userName

	// Load account key pair
	akp, err := nkeys.FromSeed([]byte(accountSeed))
	if err != nil {
		return "", fmt.Errorf("unable to create account key pair from seed: %w", err)
	}

	// Sign the user claims and convert it into a JWT string
	ujwt, err := uc.Encode(akp)
	if err != nil {
		return "", fmt.Errorf("unable to sign the claims: %w", err)
	}

	return ujwt, nil
}
```

### Notes

You can see the key (and any signing keys) of your operator using `nsc list keys --show-seeds`, you should use a 'signing key' to create the account JWTs (as singing keys can be revoked/rotated easily)

To delete accounts use the `"$SYS.REQ.CLAIMS.DELETE"` (see [reference](/running-a-nats-service/nats_admin/security/jwt.md#subjects-available-when-using-nats-based-resolver)) and make sure to enable JWT deletion in your nats-server resolver (`config allow_delete: true` in the `resolver` stanza of the server configuration).

The system is just like any other account, the only difference is that it is listed as system account in the operator's JWT (and the server config).


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/nats_admin/security.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
