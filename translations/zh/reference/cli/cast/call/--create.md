# cast call --create

忽略地址字段并模拟创建合约

```bash
$ cast call --create --help
Usage: cast call --create [OPTIONS] <CODE> [SIG] [ARGS]...

Arguments:
  <CODE>
          Bytecode of contract

  [SIG]
          The signature of the constructor

  [ARGS]...
          The arguments of the constructor

Options:
      --value <VALUE>
          Ether to send in the transaction.
          
          Either specified in wei, or as a string with a unit type.
          
          Examples: 1ether, 10gwei, 0.01ether

  -h, --help
          Print help (see a summary with '-h')
```