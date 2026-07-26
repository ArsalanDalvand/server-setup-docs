---
title: "8.9. GPG agent"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/gpg-agent.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/gpg-agent.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "8.9. GPG agent"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](ch08s08.en.md)Securing Debian Manual[Next](ch09.en.md)

## 8.9.Â GPG agent

			It is very common nowadays to digitally sign (and sometimes encrypt) e-mail. You might, for example, find that many people participating on mailing lists sign their list e-mail. Public key signatures are currently the only means to verify that an e-mail was sent by the sender and not by some other person.
		
			Debian GNU/Linux provides a number of e-mail clients with built-in e-mail signing capabilities that interoperate either with gnupg or pgp: 
			
evolution.
					
mutt.
					
kmail.
					
icedove (rebranded version of Mozilla's Thunderbird) through the [http://enigmail.mozdev.org/](http://enigmail.mozdev.org/) plugin. This plugin is provided by the enigmail package.
					
sylpheed. Depending on how the stable version of this package evolves, you may need to use the bleeding edge version, sylpheed-claws.
					
gnus, which when installed with the mailcrypt package, is an emacs interface to gnupg.
					
kuvert, which provides this functionality independently of your chosen mail user agent (MUA) by interacting with the mail transport agent (MTA).
					

			Key servers allow you to download published public keys so that you may verify signatures. One such key server is [http://wwwkeys.pgp.net](http://wwwkeys.pgp.net). gnupg can automatically fetch public keys that are not already in your public keyring. For example, to configure gnupg to use the above key server, edit the file ~/.gnupg/options and add the following line: [[58]](#ftn.idm3875)

```bash
keyserver wwwkeys.pgp.net
```

			Most key servers are linked, so that when your public key is added to one server, the addition is propagated to all the other public key servers. There is also a Debian GNU/Linux package debian-keyring, that provides all the public keys of the Debian developers. The gnupg keyrings are installed in /usr/share/keyrings/.
		
			For more information: 
			
[http://www.gnupg.org/faq.html](http://www.gnupg.org/faq.html).
					
[http://www.gnupg.org/gph/en/manual.html](http://www.gnupg.org/gph/en/manual.html).
					
[https://web.archive.org/web/20080201103530/http://www.dewinter.com/gnupg_howto/english/GPGMiniHowto.html](https://web.archive.org/web/20080201103530/http://www.dewinter.com/gnupg_howto/english/GPGMiniHowto.html).
					
[https://web.archive.org/web/20080513095235/http://www.uk.pgp.net/pgpnet/pgp-faq/](https://web.archive.org/web/20080513095235/http://www.uk.pgp.net/pgpnet/pgp-faq/).
					
[https://web.archive.org/web/20060222110131/http://www.cryptnet.net/fdp/crypto/gpg-party.html](https://web.archive.org/web/20060222110131/http://www.cryptnet.net/fdp/crypto/gpg-party.html).
					
[[58]](#idm3875)
				For more examples of how to configure gnupg check /usr/share/doc/mutt/examples/gpg.rc.
			[Prev8.8.Â Antivirus tools](ch08s08.en.md)[Up](#)[Home](index.en.md)[NextChapterÂ 9.Â Developer's Best Practices for OS Secu...](ch09.en.md)