## Forge 标准库概览

Forge Standard Library（简称 Forge Std）是一个有用的合约集合，可以让编写测试更简单、更快速、更人性化。

使用 Forge Std 是使用 Foundry 编写测试的首选方式。

它提供了开始编写测试所需的所有基本功能：

- `Vm.sol`：最新的cheatcodes界面
- `console.sol` 和 `console2.sol`：Hardhat 风格的日志记录功能
-`Script.sol`：[Solidity 脚本] 的基本实用程序(../tutorials/solidity-scripting.md)
- `Test.sol`：DSTest 的超集，包含标准库、作弊代码实例 (`vm`) 和 Hardhat 控制台

只需导入 `Test.sol` 并从测试合约中的 `Test` 继承：

```solidity
import "forge-std/Test.sol";

contract ContractTest is Test { ...
```

现在你可以：

```solidity
// Access Hevm via the `vm` instance
vm.startPrank(alice);

// Assert and log using Dappsys Test
assertEq(dai.balanceOf(alice), 10000e18);

// Log with the Hardhat `console` (`console2`)
console.log(alice.balance);

// Use anything from the Forge Std std-libraries
deal(address(dai), alice, 10000e18);
```


要单独导入 `Vm` 界面或 `console` 库：

```solidity
import "forge-std/Vm.sol";
```

```solidity
import "forge-std/console.sol";
```

**注意：** `console2.sol` 包含 `console.sol` 的补丁，允许 Forge 解码控制台调用的跟踪，但它与 `Hardhat` 不兼容。

```solidity
import "forge-std/console2.sol";
```

### 标准库

Forge Std 目前由六个标准库组成。

#### Std Logs

Std Logs扩展了 [`DSTest`](../reference/ds-test.md#logging) 库中的日志记录事件。

#### Std Assertions

Std Assertions扩展了 [`DSTest`](../reference/ds-test.md#asserting) 库中的断言函数。

#### Std Cheats

Std Cheats 是 Forge 作弊代码的包装器，使它们更安全地使用和改进 DX。

你可以通过简单地在你的测试合约中调用它们来访问 Std Cheats，就像你调用任何其他内部函数一样：

```solidity
// set up a prank as Alice with 100 ETH balance
hoax(alice, 100 ether);
```

#### Std Errors

Std Errors 提供围绕常见内部 Solidity 错误errors和恢复reverts的包装器。

Std Errors与 [`expectRevert`](../cheatcodes/expect-revert.md) cheatcodes结合使用最有用，因为您不需要自己记住内部 Solidity panic codes。 请注意，您必须通过 stdError 访问它们，因为这是一个库。

```solidity
// expect an arithmetic error on the next call (e.g. underflow)
vm.expectRevert(stdError.arithmeticError);
```

#### Std Storage

Std Storage 使操作合约存储变得容易。 它可以找到并写入与特定变量关联的存储槽。

`Test` 合约已经提供了一个 `StdStorage` 实例 `stdstore`，您可以通过它访问任何标准存储功能。 请注意，您必须先在测试合约中添加“使用 stdStorage 来存储 StdStorage”。

```solidity
// find the variable `score` in the contract `game`
// and change its value to 10
stdstore
    .target(address(game))
    .sig(game.score.selector)
    .checked_write(10);
```

#### Std Math

Std Math 是一个库，其中包含 Solidity 中未提供的有用的数学函数。

请注意，您必须通过 `stdMath` 访问它们，因为这是一个库。

```solidity
// get the absolute value of -10
uint256 ten = stdMath.abs(-10)
```

<br>

> 📚 **参考**
>
> 有关 Forge 标准库的完整概述，请参阅 [Forge 标准库参考](../reference/forge-std/)。