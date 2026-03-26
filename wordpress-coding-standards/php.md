---
title: "PHP 编码标准"
post_status: publish
comment_status: open
taxonomy:
  category:
    - wpcs-docs
  post_tag:
    - Wordpress Coding Standards
    - Repos
    - Data
---

# PHP 编码标准

这些 PHP 编码标准旨在供整个 WordPress 社区使用。它们对 WordPress 核心代码是强制性的，我们也鼓励您在主题和插件中使用它们。

虽然主题和插件可以选择遵循不同的 _编码风格_，但这些 **_编码标准_** 不仅仅是关于 _代码风格_，还包含了 WordPress 生态系统中关于互操作性、可翻译性和安全性的既定最佳实践。因此，即使使用不同的 _代码风格_，我们仍建议您在涉及这些最佳实践时遵循 WordPress 编码标准。

虽然并非所有代码都能（完全）符合这些标准，但所有新提交和/或更新的代码都应完全符合这些编码标准。

另请参阅 [PHP 内联文档标准](https://developer.wordpress.org/coding-standards/inline-documentation-standards/php/) 以获取更多指南。

如果您想根据此标准自动检查代码，可以使用官方的 [WordPress 编码标准](https://github.com/WordPress/WordPress-Coding-Standards) 工具，该工具通过 [PHP_CodeSniffer](https://github.com/PHPCSStandards/PHP_CodeSniffer/) 运行。

## General

### Opening and Closing PHP Tags

When embedding multi-line PHP snippets within an HTML block, the PHP open and close tags must be on a line by themselves.

Correct (Multiline):

```php
function foo() {
    ?>
    <div>
        <?php
        echo esc_html(
            bar(
                $baz,
                $bat
            )
        );
        ?>
    </div>
    <?php
}
```

Correct (Single Line):

```php
<input name="<?php echo esc_attr( $name ); ?>" />
```

Incorrect:

```php
if ( $a === $b ) { ?>
<some html>
<?php }
```

### 不使用简写 PHP 标签

**重要提示：** 切勿使用简写的 PHP 起始标签。请始终使用完整的 PHP 标签。

正确示例：

```php
<?php ... ?>
<?php echo esc_html( $var ); ?>
```

错误示例：

```php
<? ... ?>
<?= esc_html( $var ) ?>
```

### 单引号与双引号

在适当情况下使用单引号和双引号。如果字符串中无需解析任何内容，请使用单引号。你几乎不需要在字符串中转义引号，因为你可以交替使用引号风格，例如：

```php
echo '<a href="/static/link" class="button button-primary">Link name</a>';
echo "<a href='{$escaped_link}'>text with a ' single quote</a>";
```

放入 HTML 或 XML 属性的文本必须进行转义，避免单引号或双引号提前结束属性值导致 HTML 失效，从而引发安全问题。详见插件手册中的[数据验证](https://developer.wordpress.org/plugins/security/data-validation/)章节。

### 编写 require/include 语句

由于 `require[_once]` 和 `include[_once]` 是语言结构，路径不需要括号，因此不应使用括号。路径与 require/include 关键字之间应只有一个空格。

**强烈建议** 对无条件包含使用 `require[_once]`。当使用 `include[_once]` 时，如果文件未找到，PHP 会抛出警告但继续执行，如果应用程序依赖于加载的文件，这几乎肯定会导致其他错误/警告/通知被抛出，可能引发安全漏洞。因此，`require[_once]` 通常是更好的选择，因为如果文件未找到，它会抛出 `致命错误`。

```php
// 正确。
require_once ABSPATH . 'file-name.php';

// 错误。
include_once  ( ABSPATH . 'file-name.php' );
require_once     __DIR__ . '/file-name.php';
```

## 命名

### 命名规范

变量、动作/过滤器及函数名称应使用小写字母（切勿使用 `camelCase`）。单词间用下划线分隔。不要随意缩写变量名；确保代码清晰明确且具有自解释性。

```php
function some_name( $some_variable ) {}
```

对于函数参数命名，**强烈建议**避免使用保留关键字作为名称，因为在 PHP 8.0 的"函数调用命名参数"特性中使用时，会导致代码难以阅读且令人困惑。
同时请注意，自 PHP 8.0 起，重命名函数参数应被视为破坏性变更，因此命名函数参数时务必审慎！

类、特征、接口和枚举名称应使用大写单词并用下划线分隔。任何缩写词应全部大写。

```php
class Walker_Category extends Walker {}
class WP_HTTP {}

interface Mailer_Interface {}
trait Forbid_Dynamic_Properties {}
enum Post_Status {}
```

常量应全部大写，单词间用下划线分隔：

```php
define( 'DOING_AJAX', true );
```

文件应使用描述性小写字母命名。单词间用连字符分隔。

```php
my-plugin-name.php
```

类文件名应基于类名，前缀添加 `class-` 并将类名中的下划线替换为连字符，例如 `WP_Error` 对应文件名为：

```php
class-wp-error.php
```

此文件命名标准适用于所有当前及新增的包含类的文件，但测试类除外。
对于包含测试类的文件，根据 PSR4 规范，文件名应精确反映类名。这是为了确保跨版本的[与所有受支持 PHPUnit 版本的兼容性](https://github.com/sebastianbergmann/phpunit/pull/4109)。

`wp-includes` 目录中包含模板标签的文件应在名称末尾添加 `-template` 后缀以便识别。

```php
general-template.php
```

### Interpolation for Naming Dynamic Hooks

Dynamic hooks should be named using interpolation rather than concatenation for readability and discoverability purposes.

Dynamic hooks are hooks that include dynamic values in their tag name, e.g. `{$new_status}_{$post->post_type}` (publish_post).

Variables used in hook tags should be wrapped in curly braces `{` and `}`, with the complete outer tag name wrapped in double quotes. This is to ensure PHP can correctly parse the given variables' types within the interpolated string.

```php
do_action( "{$new_status}_{$post->post_type}", $post->ID, $post );
```

Where possible, dynamic values in tag names should also be as succinct and to the point as possible. `$user_id` is much more self-documenting than, say, `$this->id`.

## Whitespace

### Space Usage

Always put spaces after commas, and on both sides of logical, arithmetic, comparison, string and assignment operators.

```php
SOME_CONST === 23;
foo() && bar();
! $foo;
array( 1, 2, 3 );
$baz . '-5';
$term .= 'X';
if ( $object instanceof Post_Type_Interface ) {}
$result = 2 ** 3; // 8.
```

Put spaces on both sides of the opening and closing parentheses of control structure blocks.

```php
foreach ( $foo as $bar ) { ...
```

When defining a function, do it like so:

```php
function my_function( $param1 = 'foo', $param2 = 'bar' ) { ...

function my_other_function() { ...
```

When calling a function, do it like so:

```php
my_function( $param1, func_param( $param2 ) );
my_other_function();
```

When performing logical comparisons, do it like so:

```php
if ( ! $foo ) { ...
```

[Type casts](https://www.php.net/manual/en/language.types.type-juggling.php#language.types.typecasting) must be lowercase. Always prefer the short, canonical form of type casts, `(int)` instead of `(integer)` and `(bool)` rather than `(boolean)`. For float casts use `(float)`.

```php
$foo = (bool) $bar; // Correct.
$foo = (boolean) $bar; // Incorrect.
```

When referring to array items, only include a space around the index if it is a variable, for example:

```php
$x = $foo['bar']; // Correct.
$x = $foo[ 'bar' ]; // Incorrect.

$x = $foo[0]; // Correct.
$x = $foo[ 0 ]; // Incorrect.

$x = $foo[ $bar ]; // Correct.
$x = $foo[$bar]; // Incorrect.
```

In a `switch` block, there must be no space between the `case` condition and the colon.

```php
switch ( $foo ) {
    case 'bar': // Correct.
    case 'bar' : // Incorrect.
}
```

Unless otherwise specified, parentheses should have spaces inside them.

```php
if ( $foo && ( $bar || $baz ) ) { ...

my_function( ( $x - 1 ) * 5, $y );
```

When using increment (`++`) or decrement (`--`) operators, there should be no spaces between the operator and the variable it applies to.

```php
// Correct.
for ( $i = 0; $i < 10; $i++ ) {}

// Incorrect.
for ( $i = 0; $i < 10; $i ++ ) {}
++   $b; // Multiple spaces.
```

### 缩进

缩进应始终反映逻辑结构。使用**真正的制表符**，**而非空格**，因为这在各客户端间具有最佳灵活性。

例外情况：如果某段代码通过对齐能提升可读性，可使用空格：

```php
[tab]$foo   = 'somevalue';
[tab]$foo2  = 'somevalue2';
[tab]$foo34 = 'somevalue3';
[tab]$foo5  = 'somevalue4';
```

对于包含显式键名的数组，当数组包含多个元素时，_每个元素_都应在新行开始：

```php
$query = new WP_Query( array( 'ID' => 123 ) );
```

```php
$args = array(
[tab]'post_type'   => 'page',
[tab]'post_author' => 123,
[tab]'post_status' => 'publish',
);

$query = new WP_Query( $args );
```

注意最后一个数组元素后的逗号：建议保留此逗号，因为这便于调整数组顺序，且在添加新元素时能生成更清晰的差异对比。

```php
$my_array = array(
[tab]'foo'   => 'somevalue',
[tab]'foo2'  => 'somevalue2',
[tab]'foo3'  => 'somevalue3',
[tab]'foo34' => 'somevalue3',
);
```

对于 `switch` 控制结构，`case` 语句应比 `switch` 语句缩进一个制表符，而 `case` 内的内容应比 `case` 条件语句再缩进一个制表符。

```php
switch ( $type ) {
[tab]case 'foo':
[tab][tab]some_function();
[tab][tab]break;
[tab]case 'bar':
[tab][tab]some_function();
[tab][tab]break;
}
```

**经验法则：** 行首应使用制表符进行缩进，而行中可使用空格进行对齐。

### 移除尾部空格

移除每行末尾的尾部空白。建议省略文件末尾的 PHP 闭合标签。如果使用了该标签，请确保移除其后的尾部空白。

函数体末尾不应存在尾部空行。

## 格式规范

### 大括号风格

所有代码块都应使用如下所示的大括号风格：

```php
if ( condition ) {
    action1();
    action2();
} elseif ( condition2 && condition3 ) {
    action3();
    action4();
} else {
    defaultaction();
}
```

如果代码块过长，应考虑是否能将其拆分为两个或多个较短的代码块、函数或方法，以降低复杂度、提高测试便利性并增强可读性。

即使在不强制要求的情况下，也应始终使用大括号：

```php
if ( condition ) {
    action0();
}

if ( condition ) {
    action1();
} elseif ( condition2 ) {
    action2a();
    action2b();
}

foreach ( $items as $item ) {
    process_item( $item );
}
```

请注意，强制使用大括号意味着**禁止单语句内联控制结构**。你可以自由使用[控制结构的替代语法](https://www.php.net/manual/en/control-structures.alternative-syntax.php)（例如 `if`/`endif`、`while`/`endwhile`）——尤其是在模板中嵌入 PHP 代码时，例如：

```php
<?php if ( have_posts() ) : ?>
    <div class="hfeed">
        <?php while ( have_posts() ) : the_post(); ?>
            <article id="<?php echo esc_attr( 'post-' . get_the_ID() ); ?>" class="<?php echo esc_attr( get_post_class() ); ?>">
                <!-- ... -->
            </article>
        <?php endwhile; ?>
    </div>
<?php endif; ?>
```

### 声明数组

使用长数组语法（`array( 1, 2, 3 )`）声明数组通常比短数组语法（`[ 1, 2, 3 ]`）更具可读性，尤其对于有视力障碍者。此外，它对初学者来说也更具描述性。

数组必须使用长数组语法进行声明。

### 多行函数调用

当函数调用需要跨越多行时，每个参数必须独占一行。单行内联注释可以单独占据一行。

每个参数不得超过一行。多行参数值必须先赋值给变量，然后将该变量传递给函数调用。

```php
$bar = array(
    'use_this' => true,
    'meta_key' => 'field_name',
);
$baz = sprintf(
    /* translators: %s: 朋友的名字 */
    __( 'Hello, %s!', 'yourtextdomain' ),
    $friend_name
);

$a = foo(
    $bar,
    $baz,
    /* translators: %s: 猫 */
    sprintf( __( 'The best pet is a %s.' ), 'cat' )
);
```

### Type declarations

Type declarations must have exactly one space before and after the type. The nullability operator (`?`) is regarded as part of the type declaration and there should be no space between this operator and the actual type. Class/interface/enum name based type declarations should use the case of the class/interface/enum name as declared, while the keyword-based type declarations should be lowercased.

Return type declarations should have no space between the closing parenthesis of the function declaration and the colon starting a return type.

These rules apply to all structures allowing for type declarations: functions, closures, enums, catch conditions as well as the PHP 7.4 arrow functions and typed properties.

```php
// Correct.
function foo( Class_Name $parameter, callable $callable, int $number_of_things = 0 ) {
    // Do something.
}

function bar(
    Interface_Name&Concrete_Class $param_a,
    string|int $param_b,
    callable $param_c = 'default_callable'
): User|false {
    // Do something.
}

// Incorrect.
function baz(Class_Name $param_a, String$param_b,      CALLABLE     $param_c )   :   ?   iterable   {
    // Do something.
}
```

[info]
Type declaration usage has the following restrictions based on the minimum required PHP version of an application, whether it is WordPress Core, a plugin or a theme:

- The scalar `bool`, `int`, `float`, and `string` type declarations cannot be used until the minimum PHP version is PHP 7.0 or higher.
- Return type declarations cannot be used until the minimum PHP version is PHP 7.0 or higher.
- Nullable type declarations cannot be used until the minimum PHP version is PHP 7.1 or higher.
- The `iterable` and `void` type declarations cannot be used until the minimum PHP version is PHP 7.1 or higher. The `void` type can only be used as a return type.
- The `object` type declaration cannot be used until the minimum PHP version is PHP 7.2 or higher.
- Property type declarations cannot be used until the minimum PHP version is PHP 7.4 or higher.
- `static` (return type only) cannot be used until the minimum PHP version is PHP 8.0 or higher.
- `mixed` type cannot be used until the minimum PHP version is PHP 8.0 or higher. Take note that the `mixed` type includes `null`, so cannot be made nullable.
- Union types cannot be used until the minimum PHP version is PHP 8.0 or higher.
- Intersection types cannot be used until the minimum PHP version is PHP 8.1 or higher.
- `never` (return type only) cannot be used until the minimum PHP version is PHP 8.1 or higher.
- Disjunctive normal form types (combining union types and intersection types) cannot be used until the minimum PHP version is PHP 8.2 or higher.
[/info]

_Usage in WordPress Core_

[warning]
Adding type declarations to existing WordPress Core functions should be done with extreme care.
[/warning]

任何可由插件或主题重载的函数（方法）签名都不应被改动。
目前，这仅留下全局命名空间中无条件声明的函数、`private`类方法以及核心代码新增部分，作为添加类型声明的候选对象。

注意：目前**强烈不建议**在类型声明中使用`array`关键字，因为大多数情况下，使用`iterable`能提供更灵活的实现，但在WordPress核心代码中，该关键字需等到最低要求提升至PHP 7.1后方可使用。

### 魔术常量

[PHP 原生 `__*__` 魔术常量](https://www.php.net/manual/en/language.constants.magic.php)，例如 `__CLASS__` 和 `__DIR__`，在使用时应全部大写。

当使用 `::class` 常量进行类名解析时，`class` 关键字应为小写，且 `::` 运算符前后不应有空格。

```php
// 正确。
add_action( 'action_name', array( __CLASS__, 'method_name' ) );
add_action( 'action_name', array( My_Class::class, 'method_name' ) );

// 错误。
require_once __dIr__ . '/relative-path/file-name.php';
add_action( 'action_name', array( My_Class :: CLASS, 'method_name' ) );
```

### 展开运算符 `...`

使用展开运算符时，应在展开运算符前保留一个空格或带适当缩进的换行。展开运算符与其应用的变量/函数调用之间不应有空格。当展开运算符与引用运算符（`&`）结合使用时，两者之间不应有空格。

```php
// 正确示例。
function foo( &...$spread ) {
    bar( ...$spread );

    bar(
        array( ...$foo ),
        ...array_values( $keyed_array )
    );
}

// 错误示例。
function fool( &   ... $spread ) {
    bar(...
             $spread );

    bar(
        [...  $foo ],... array_values( $keyed_array )
    );
}
```

[info]
展开运算符（在其他语言中称为 splat 运算符）自 PHP 5.6 起可用于函数声明中的参数打包（可变参数函数）及函数调用中的参数解包。自 PHP 7.4 起，展开运算符还可用于解包数字索引数组，自 PHP 8.1 起支持字符串键数组的解包。
在函数声明中使用时，展开运算符仅能用于最后一个参数。
[/info]

## Declare Statements, Namespace, and Import Statements

### Namespace declarations

Each part of a namespace name should consist of capitalized words separated by underscores.

Namespace declarations should have exactly one blank line before the declaration and at least one blank line after.

```php
namespace Prefix\Admin\Domain_URL\Sub_Domain\Event; // Correct.
```

There should be only one namespace declaration per file, and it should be at the top of the file. Namespace declarations using curly brace syntax are not allowed. Explicit global namespace declaration (namespace declaration without name) are also not allowed.

```php
// Incorrect: namespace declaration using curly brace syntax.
namespace Foo {
    // Code.
}

// Incorrect: namespace declaration for the global namespace.
namespace {
    // Code.
}
```

_There is currently no timeline for introducing namespaces to WordPress Core._

The use of namespaces in plugins and themes is strongly encouraged. It is a great way to prefix a lot of your code to prevent naming conflicts with other plugins, themes and/or WordPress Core.

Please do make sure you use a unique and long enough namespace prefix to actually prevent conflicts. Generally speaking, using a namespace prefix along the lines of `Vendor\Project_Name` is a good idea.

[warning]
The `wp` and `WordPress` namespace prefixes are reserved for WordPress itself.
[/warning]

[info]
Namespacing has no effect on variables, constants declared with `define()` or non-PHP native constructs, like the hook names as used in WordPress.
Those still need to be prefixed individually.
[/info]

### Using import `use` statements

Using import `use` statements allows you to refer to constants, functions, classes, interfaces, namespaces, enums and traits that live outside of the current namespace.

Import `use` statements should be at the top of the file and follow the (optional) `namespace` declaration. They should follow a specific order based on the **type** of the import:

1. `use` statements for namespaces, classes, interfaces, traits and enums
2. `use` statements for functions
3. `use` statements for constants

Aliases can be used to prevent name collisions (two classes in different namespaces using the same class name).
When using aliases, make sure the aliases follow the WordPress naming convention and are unique.

The following examples showcase the correct and incorrect usage of import `use` statements regarding things like spacing, groupings, leading backslashes, etc.

Correct:

```php
namespace Project_Name\Feature;

use Project_Name\Sub_Feature\Class_A;
use Project_Name\Sub_Feature\Class_C as Aliased_Class_C;
use Project_Name\Sub_Feature\{
    Class_D,
    Class_E as Aliased_Class_E,
}

use function Project_Name\Sub_Feature\function_a;
use function Project_Name\Sub_Feature\function_b as aliased_function;

use const Project_Name\Sub_Feature\CONSTANT_A;
use const Project_Name\Sub_Feature\CONSTANT_D as ALIASED_CONSTANT;

// Rest of the code.
```

Incorrect:

```php
namespace Project_Name\Feature;

use   const   Project_Name\Sub_Feature\CONSTANT_A; // Superfluous whitespace after the "use" and the "const" keywords.
use function Project_Name\Sub_Feature\function_a; // Function import after constant import.
use \Project_Name\Sub_Feature\Class_C as aliased_class_c; // Leading backslash shouldn't be used, alias doesn't comply with naming conventions.
use Project_Name\Sub_Feature\{Class_D, Class_E   as   Aliased_Class_E} // Extra spaces around the "as" keyword, incorrect whitespace use inside the brace opener and closer.
use Vendor\Package\{ function function_a, function function_b,
     Class_C,
        const CONSTANT_NAME}; // Combining different types of imports in one use statement, incorrect whitespace use within group use statement.

class Foo {
    // Code.
}

use const \Project_Name\Sub_Feature\CONSTANT_D as Aliased_constant; // Import after class definition, leading backslash, naming conventions violation.
use function Project_Name\Sub_Feature\function_b as Aliased_Function; // Import after class definition, naming conventions violation.

// Rest of the code.
```

[alert]
Import `use` statements have no effect on dynamic class, function or constant names.
Group `use` statements are available from PHP 7.0, and trailing commas in group `use` statements are available from PHP 7.2.
[/alert]

[info]
请注意，除非您已实现[自动加载](https://www.php.net/manual/en/language.oop5.autoload.php)，否则 `use` 语句不会自动加载任何导入的内容。对于面向对象结构，您需要设置自动加载，或使用 `require[_once]` 或 `include[_once]` 语句加载包含面向对象声明的文件。
自动加载仅适用于面向对象结构；对于函数和常量，您必须始终使用 `require[_once]` 或 `include[_once]`。
[/info]

**关于 WordPress 核心使用的说明**

尽管导入 `use` 语句已可在 WordPress 核心中使用，但目前**强烈不建议使用**。

导入 `use` 语句在与命名空间和类自动加载实现结合时最为有用。
由于 WordPress 核心目前尚未采用这些机制，且相关讨论仍在进行中，暂时不在 WordPress 核心中添加导入 `use` 语句是明智的选择。

## 面向对象编程

### 每个文件仅包含一个对象结构（类/接口/特征/枚举）

例如，如果我们有一个名为 `class-example-class.php` 的文件，则该文件只能包含一个类。

```php
// 错误示例：文件 class-example-class.php。
class Example_Class {}

class Example_Class_Extended {}
```

第二个类应放在其自己的文件中，文件名为 `class-example-class-extended.php`。

```php
// 正确示例：文件 class-example-class.php。
class Example_Class {}
```

```php
// 正确示例：文件 class-example-class-extended.php。
class Example_Class_Extended {}
```

### Trait Use Statements

Trait `use` statements should be at the top of a class and should have exactly one blank line before the first `use` statement, and at least one blank line after the last statement. The only exception is when the class only contains trait `use` statements, in which case the blank line after may be omitted.

The following code examples show the formatting requirements for trait `use` statements regarding things like spacing, grouping and indentation.

```php
// Correct.
class Foo {

    use Bar_Trait;
    use Foo_Trait,
        Bazinga_Trait {
        Bar_Trait::method_name insteadof Bar_Trait;
        Bazinga_Trait::method_name as bazinga_method;
    }
    use Loopy_Trait {
        eat as protected;
    }

    public $baz = true;

    ...
}

// Incorrect.
class Foo {
    // No blank line before trait use statement, multiple spaces after the use keyword.
    use       Bar_Trait;

    /*
     * Multiple spaces when importing traits, no new line after opening brace.
     * Aliasing should be done on the same line as the method it's replacing.
     */
    use Foo_Trait,   Bazinga_Trait{Bar_Trait::method_name    insteadof     Foo_Trait; Bazinga_Trait::method_name
        as     bazinga_method;
            }; // Wrongly indented brace.
    public $baz = true; // Missing blank line after trait import.

    ...
}
```

### 必须始终声明可见性

对于所有支持可见性声明的结构（属性、方法、自 PHP 7.1 起的类常量），都应明确声明其可见性。
禁止使用 `var` 关键字声明属性。

```php
// 正确示例。
class Foo {
    public $foo;

    protected function bar() {}
}

// 错误示例。
class Foo {
    var $foo;

    function bar() {}
}
```

_在 WordPress 核心代码中的使用_

在 WordPress 核心代码中，直到最低 PHP 版本要求提升至 PHP 7.1 之前，类常量不可使用可见性声明。

### Visibility and modifier order

When using multiple modifiers for a _class declaration_, the order should be as follows:

1. First the optional `abstract` or `final` modifier.
2. Next, the optional `readonly` modifier.

When using multiple modifiers for a _constant declaration_ inside object-oriented structures, the order should be as follows:

1. First the optional `final` modifier.
2. Next, the visibility modifier.

When using multiple modifiers for a _property declaration_, the order should be as follows:

1. First a visibility modifier.
2. Next, the optional `static` or `readonly` modifier (these keywords are mutually exclusive).
3. Finally, the optional `type` declaration.

When using multiple modifiers for a _method declaration_, the order should be as follows:

1. First the optional `abstract` or `final` modifier.
2. Then, a visibility modifier.
3. Finally, the optional `static` modifier.

```php
// Correct.
abstract readonly class Foo {
    private const LABEL = 'Book';

    public static $foo;

    private readonly string $bar;

    abstract protected static function bar();
}

// Incorrect.
class Foo {
    protected final const SLUG = 'book';

    static public $foo;

    static protected final function bar() {
        // Code.
    };
}
```

[info]
- Visibility for OO constants can be declared since PHP 7.1.
- Typed properties are available since PHP 7.4.
- Readonly properties are available since PHP 8.1.
- `final` modifier for constants in object-oriented structures is available since PHP 8.1.
- Readonly classes are available since PHP 8.2.
[/info]

### 对象实例化

当实例化新对象时，必须始终使用括号，即使严格来说并非必要。
被实例化的类名与左括号之间不应有空格。

```php
// 正确。
$foo = new Foo();
$anonymous_class = new class( $parameter ) { ... };
$instance = new static();

// 错误。
$foo = new Foo;
$anonymous_class = new class ( $input ) { ... };
```

## 控制结构

### 使用 `elseif`，而非 `else if`

`else if` 与 `if|elseif` 代码块的冒号语法不兼容。因此，在条件语句中请使用 `elseif`。

### Yoda Conditions

```php
if ( true === $the_force ) {
    $victorious = you_will( $be );
}
```

When doing logical comparisons involving variables, always put the variable on the right side and put constants, literals, or function calls on the left side. If neither side is a variable, the order is not important. (In [computer science terms](https://en.wikipedia.org/wiki/Value_(computer_science)#Assignment:_l-values_and_r-values), in comparisons always try to put l-values on the right and r-values on the left.)

In the above example, if you omit an equals sign (admit it, it happens even to the most seasoned of us), you'll get a parse error, because you can't assign to a constant like `true`. If the statement were the other way around `( $the_force = true )`, the assignment would be perfectly valid, returning `1`, causing the if statement to evaluate to `true`, and you could be chasing that bug for a while.

A little bizarre, it is, to read. Get used to it, you will.

This applies to `==`, `!=`, `===`, and `!==`. Yoda conditions for `<`, `>`, `<=` or `>=` are significantly more difficult to read and are best avoided.

## 运算符

### 三元运算符

[三元运算符](https://www.php.net/manual/en/language.operators.comparison.php#language.operators.comparison.ternary)可以使用，但应始终测试语句是否为真，而非为假。否则容易造成混淆。（例外情况是使用 `! empty()`，因为在此测试是否为假通常更直观。）

禁止使用短三元运算符。

例如：

```php
// (如果语句为真) ? (执行此操作) : (否则，执行此操作);
$musictype = ( 'jazz' === $music ) ? 'cool' : 'blah';
// (如果字段不为空) ? (执行此操作) : (否则，执行此操作);
```

### 错误控制运算符 `@`

如 [PHP 文档](https://www.php.net/manual/en/language.operators.errorcontrol.php) 所述：

> PHP 支持一个错误控制运算符：@ 符号。当在 PHP 中前置到表达式前时，该表达式可能生成的任何诊断错误都将被抑制。

尽管此运算符确实存在于核心中，但它常被随意使用，而不是进行适当的错误检查。强烈不鼓励使用它，因为 PHP 文档也指出：

> 警告：在 PHP 8.0.0 之前，@ 运算符有可能禁用会导致脚本执行终止的关键错误。例如，在调用不存在的函数（由于不可用或拼写错误）前加上 @，会导致脚本终止且没有任何指示原因。

### 自增/自减运算符

对于独立语句，应优先使用前自增/自减而非后自增/自减。

前自增/自减会先执行增减操作再返回值，而后自增/自减会先返回值再执行增减操作。
使用"前"版本性能稍优，且在代码移动时能避免潜在的bug。

```php
// 正确：前自减。
--$a;

// 错误：后自减。
$a--;
```

## 数据库

### 数据库查询

避免直接操作数据库。如果已有定义好的函数能获取所需数据，请使用它。数据库抽象化（使用函数而非查询）有助于保持代码的前向兼容性，并且在结果缓存在内存中的情况下，速度可能快上许多倍。

如果必须操作数据库，请考虑创建一个 [Trac](https://core.trac.wordpress.org/) 工单。在那里，你可以讨论为未来版本的 WordPress 添加新函数以实现所需功能的可能性。

### 格式化 SQL 语句

在格式化 SQL 语句时，如果语句足够复杂，可以将其分成多行并适当缩进。不过大多数语句单行即可清晰表达。请始终将 SQL 关键字部分大写，例如 `UPDATE` 或 `WHERE`。

更新数据库的函数应预期其参数在传递时未经过 SQL 斜杠转义。转义操作应尽可能在接近查询执行时进行，最好使用 `$wpdb->prepare()` 方法。

`$wpdb->prepare()` 是一个为 SQL 查询处理转义、引号包裹和整数类型转换的方法。它采用 `sprintf()` 风格格式的子集。示例：

```php
$var = "dangerous'"; // 可能需要转义的原始数据
$id = some_foo_number(); // 预期为整数的数据，但不确定其类型

$wpdb->query( $wpdb->prepare( "UPDATE $wpdb->posts SET post_title = %s WHERE ID = %d", $var, $id ) );
```

查询字符串中可使用以下占位符：

- %d（整数）
- %f（浮点数）
- %s（字符串）
- %i（标识符，如表名/字段名）

请注意这些占位符不应被“引号包裹”！`$wpdb->prepare()` 会为我们处理转义和引号添加。这使得我们能够直观判断数据是否已转义，因为转义操作恰在查询发生时执行。

更多详细信息请参阅插件手册中的[数据验证](https://developer.wordpress.org/plugins/security/data-validation/)。

## 建议

### 函数参数的自解释标志值

在调用函数时，优先使用字符串值，而不仅仅是 `true` 和 `false`。

```php
// 错误示例。
function eat( $what, $slowly = true ) {
...
}
eat( 'mushrooms' );
eat( 'mushrooms', true ); // true 是什么意思？
eat( 'dogfood', false ); // false 是什么意思？是 true 的反义吗？
```

PHP 从 8.0 版本才开始支持命名参数。然而，由于 WordPress 目前仍支持较旧的 PHP 版本，我们还不能使用该特性。
在没有命名参数的情况下，标志值是没有意义的，每次遇到像上面示例那样的函数调用时，我们都必须查找函数定义。通过使用描述性的字符串值来代替布尔值，可以使代码更具可读性。

```php
// 正确示例。
function eat( $what, $speed = 'slowly' ) {
...
}
eat( 'mushrooms' );
eat( 'mushrooms', 'slowly' );
eat( 'dogfood', 'quickly' );
```

当需要更多词语来描述函数参数时，使用 `$args` 数组可能是更好的模式。

```php
function eat( $what, $args ) {
...
}
eat ( 'noodles', array( 'speed' => 'moderate' ) );
```

使用此模式时要小心，因为如果在使用前未验证输入，可能会导致“未定义数组索引”的提示。仅在合理的情况下（即存在多个可能的参数）使用此模式，不要为了使用而使用。

### Clever Code

In general, readability is more important than cleverness or brevity.

```php
isset( $var ) || $var = some_function();
```

Although the above line is clever, it takes a while to grok if you're not familiar with it. So, just write it like this:

```php
if ( ! isset( $var ) ) {
    $var = some_function();
}
```

Unless absolutely necessary, loose comparisons should not be used, as their behavior can be misleading.

Correct:

```php
if ( 0 === strpos( $text, 'WordPress' ) ) {
    echo esc_html__( 'Yay WordPress!', 'textdomain' );
}
```

Incorrect:

```php
if ( 0 == strpos( 'WordPress', 'foo' ) ) {
    echo esc_html__( 'Yay WordPress!', 'textdomain' );
}
```

Assignments must not be placed in conditionals.

Correct:

```php
$data = $wpdb->get_var( '...' );
if ( $data ) {
    // Use $data.
}
```

Incorrect:

```php
if ( $data = $wpdb->get_var( '...' ) ) {
    // Use $data.
}
```

In a `switch` statement, it's okay to have multiple empty cases fall through to a common block. If a case contains a block, then falls through to the next block, however, this must be explicitly commented.

```php
switch ( $foo ) {
    case 'bar':                // Correct, an empty case can fall through without comment.
    case 'baz':
        echo esc_html( $foo ); // Incorrect, a case with a block must break, return, or have a comment.
    case 'cat':
        echo 'mouse';
        break;                 // Correct, a case with a break does not require a comment.
    case 'dog':
        echo 'horse';
        // no break            // Correct, a case can have a comment to explicitly mention the fall through.
    case 'fish':
        echo 'bird';
        break;
}
```

The `goto` statement must never be used.

The `eval()` construct is _very dangerous_ and is impossible to secure. It must not be used.

The `create_function()` function internally performs an `eval()`. It was deprecated in PHP 7.2 and removed in PHP 8.0. It must not be used.

### 闭包（匿名函数）

在适当的情况下，闭包可以作为创建新函数作为回调传递的替代方案。例如：

```php
$caption = preg_replace_callback(
    '/<[a-zA-Z0-9]+(?: [^<>]+>)*/',
    function ( $matches ) {
        return preg_replace( '/[\r\n\t]+/', ' ', $matches[0] );
    },
    $caption
);
```

闭包不应作为过滤器或动作回调传递，因为通过 `remove_action()` / `remove_filter()` 移除这些回调（目前）较为复杂（有关解决此问题的提案，请参见 [#46635](https://core.trac.wordpress.org/ticket/46635 "Improve identifying of non–trivial callbacks in hooks")）。

### 正则表达式

应使用 Perl 兼容正则表达式（[PCRE](https://www.php.net/pcre)，`preg_` 系列函数）。
切勿使用 `/e` 修饰符，应改用 `preg_replace_callback`。

使用单引号字符串定义正则表达式最为便捷，因为与双引号字符串不同，单引号字符串仅需对两种元序列进行转义：`\'` 和 `\\`。

### 请勿使用 `extract()` 函数

根据 [#22400](https://core.trac.wordpress.org/ticket/22400 "移除 WordPress 中所有或至少大部分 extract() 的使用") 的说明：

> `extract()` 是一个糟糕的函数，它会使代码更难以调试和理解。我们应阻止其使用，并移除所有相关用法。

Joseph Scott 在[这篇文章中详细阐述了其弊端](https://blog.josephscott.org/2009/02/05/i-dont-like-phps-extract-function/)。

### Shell 命令

禁止使用[反引号运算符](https://www.php.net/manual/en/language.operators.execution.php)。

使用反引号运算符等同于调用 [`shell_exec()`](https://www.php.net/manual/en/function.shell-exec.php)，出于安全考虑，大多数主机都会在 `php.ini` 文件中禁用此函数。