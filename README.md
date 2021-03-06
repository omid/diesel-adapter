# diesel-adapter

[![Crates.io](https://img.shields.io/crates/v/diesel-adapter.svg)](https://crates.io/crates/diesel-adapter)
[![Docs](https://docs.rs/diesel-adapter/badge.svg)](https://docs.rs/diesel-adapter)
[![Build Status](https://travis-ci.org/casbin-rs/diesel-adapter.svg?branch=master)](https://travis-ci.org/casbin-rs/diesel-adapter)

An adapter designed to work with [casbin-rs](https://github.com/casbin/casbin-rs).


## Install

Add it to `Cargo.toml`

```
casbin = { version = "0.2.0" }
diesel-adapter = { version = "0.2.0", features = ["postgres"] }
async-std = "1.5.0"
```


## Example

```rust
use casbin::{Enforcer, FileAdapter, Model};
use diesel_adapter::{DieselAdapter, ConnOptions};
use async_std::task;

task::block_on(async {
    let mut m = Model::from_file("examples/rbac_model.conf").await?;

    let mut conn_opts = ConnOptions::default();
    conn_opts
        .set_hostname("127.0.0.1")
        .set_port(3306)
        .set_table("casbin_rules")
        .set_auth("casbin_rs", "casbin_rs");

    let a = Box::new(DieselAdapter::new(conn_opts).await?);
    let mut e = Enforcer::new(m, a).await?;
});
```

## Features

- `postgres`
- `mysql`

*Attention*: `postgres` and `mysql` are mutual exclusive which means that you can only activate one of them. Currently we don't have support for `sqlite`, it may be added in the near future.
