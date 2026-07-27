---
title: "Software Development Kits (SDK) — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/developers/minio-drivers.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "developers/minio-drivers"
category: "developers"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Software Development Kits (SDK) — SILO (MinIO Object Storage)"
---
# Software Development Kits (SDK)

MinIO publishes the following Software Development Kits (SDK):

[Go](#go-sdk)

[Python](#python-sdk)

[Java](#java-sdk)

[.NET](#dotnet-sdk)

[JavaScript](#javascript-sdk)

[Haskell](#haskell-sdk)

[C++](#cpp-sdk)

[Rust](#rust-sdk)

## Go

GitHub: [minio/minio-go](https://github.com/minio/minio-go)

Latest Version: v7.2.0

Quickstart Guide: [[developers/go/minio-go|Go Quickstart Guide]]

Reference: [[developers/go/API|MinIO Go Client API Reference]]

Download from GitHub

```
go get github.com/minio/minio-go/v7
```

## Python

GitHub: [minio/minio-py](https://github.com/minio/minio-py)

Latest Version: 7.2.20

Quickstart Guide: [[developers/python/minio-py|Python Quickstart Guide]]

Reference: [[developers/python/API|Python Client API Reference]]

Install Methods

pip

```
pip3 install minio
```

source

```
git clone https://github.com/minio/minio-py
cd minio-py
python setup.py install
```

## Java

GitHub: [minio/minio-java](https://github.com/minio/minio-java)

Latest version: 9.0.3

Quickstart Guide: [[developers/java/minio-java#minio-java-quickstart|Java Quickstart Guide]]

Reference: [[developers/java/API|Java Client API Reference]]

Install methods

Maven

```
<dependency>
    <groupId>io.minio</groupId>
    <artifactId>minio</artifactId>
    <version>9.0.3</version>
</dependency>
```

Gradle

```
dependencies {
    implementation("io.minio:minio:9.0.3")
}
```

JAR

Download the latest JAR file for version 9.0.3 of the SDK from the [Sonatype Maven Central Repository](|java-jar-url|).

## .NET

GitHub: [minio/minio-dotnet](https://github.com/minio/minio-dotnet)

Latest Version: 7.0.0

Quickstart Guide: [[developers/dotnet/minio-dotnet|.NET Quickstart Guide]]

Reference: [[developers/dotnet/API|.NET Client API Reference]]

Download from NuGet

Run the following command in the NuGet Package Manager Console.

```
PM> Install-Package Minio
```

## JavaScript

GitHub: [minio/minio-js](https://github.com/minio/minio-js)

Latest Version: 8.0.7

Quickstart Guide: [[developers/javascript/minio-javascript|JavaScript Quickstart Guide]]

Reference: [[developers/javascript/API|JavaScript Client API Reference]]

Install

NPM

```
npm install --save minio
```

Source

```
git clone https://github.com/minio/minio-js
cd minio-js
npm install
npm install -g
```

## Haskell

GitHub: [minio/minio-hs](https://github.com/minio/minio-hs)

Latest Version: v1.7.0

Quickstart Guide: [[developers/haskell/minio-haskell|Haskell Quickstart Guide]]

Install

Add `minio-hs` to your project’s `.cabal` dependencies section.

or

If you are using `hpack`, add `minio-hs` to your `package.yaml` file.

## C++

GitHub: [minio/minio-cpp](https://github.com/minio/minio-cpp)

Reference: [MinIO C++ SDK Reference](https://minio-cpp.min.io/)

Install

`vcpkg`

```
vcpkg install minio-cpp
```

Source

```
git clone https://github.com/minio/minio-cpp
cd minio-cpp
wget --quiet -O vcpkg-master.zip https://github.com/microsoft/vcpkg/archive/refs/heads/master.zip
unzip -qq vcpkg-master.zip
./vcpkg-master/bootstrap-vcpkg.sh
./vcpkg-master/vcpkg integrate install
cmake -B ./build -DCMAKE_BUILD_TYPE=Debug -DCMAKE_TOOLCHAIN_FILE=./vcpkg-master/scripts/buildsystems/vcpkg.cmake
cmake --build ./build --config Debug
```

## Rust

GitHub: [minio/minio-rs](https://github.com/minio/minio-rs)

Latest Version

v0.4.0

Reference: [MinIO Rust SDK Reference](https://docs.rs/minio/latest/minio/)

Quickstart Guide: [[developers/rust/minio-rust|Rust Quickstart Guide]]

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).