# Login to internal server

## ADD SSH KEYGEN
```sh
ssh-add -K {{KEY}}
```

## LOGIN TO JUMP SERVER
```sh
ssh -A {{USERNAME}}@{{IP}}
```

## LOGIN TO INTERNAL SERVER
```sh
ssh {{USERNAME}}@{{IP}}
```
