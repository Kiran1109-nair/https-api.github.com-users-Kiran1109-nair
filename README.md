41 lines (32 sloc)  734 Bytes
  
name: Build

on:
  push:
    branches: [develop]
    tags-ignore:
      - '**'

  pull_request:
    branches: [develop]

jobs:
  Verify:
    runs-on: ubuntu-latest

    env:
      GOPATH: /home/runner/go

    steps:
    - uses: actions/checkout@v2
      with:
        fetch-depth: 0

    - name: Set up Go
      uses: actions/setup-go@v2
      with:
        go-version: 1.16

    - name: Cache Godel assets
      uses: actions/cache@v2
      with:
        path: ~/.godel
        key: ${{ runner.os }}-godel-${{ hashFiles('godelw', 'godel/config/godel.yml') }}
        restore-keys: |
          ${{ runner.os }}-godel-
    - name: Verify
      run: ./godelw verify --apply=false --skip-test

    - name: Test
      run: ./godelw test
