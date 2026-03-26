---
title: "HTML 编码规范"
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

# HTML 编码规范

## HTML

### 验证

所有 HTML 页面都应通过 [W3C 验证器](https://validator.w3.org/) 进行验证，以确保标记结构良好。这本身并不能直接证明代码质量优秀，但有助于剔除可通过自动化测试发现的问题。它不能替代人工代码审查。（关于其他验证器，请参阅 Codex 中的 [HTML 验证](https://codex.wordpress.org/Validating_a_Website#HTML_-_Validation)。）

### 自闭合元素

所有标签都必须正确闭合。对于可以包裹文本或其他元素的标签，闭合操作相对简单。对于自闭合标签，斜杠前应恰好有一个空格：

```html
<br />
```

而不是紧凑但不正确的：

```html
<br/>
```

W3C 规定自闭合斜杠前应有一个空格（[来源](https://w3.org/TR/xhtml1/#C_2)）。

### 属性与标签

所有标签和属性必须使用小写字母书写。此外，当属性值的文本仅用于机器解析时，其值也应使用小写。若数据需要人类可读，则应遵循正确的标题大小写规则。

机器解析示例：

```html
<meta http-equiv="content-type" content="text/html; charset=utf-8" />
```

人类阅读示例：

```html
<a href="http://example.com/" title="Description Here">Example.com</a>
```

### 引号使用规范

根据 W3C 的 XHTML 规范，所有属性都必须有值，且必须使用双引号或单引号包裹（[来源](https://www.w3.org/TR/xhtml1/#h-4.4)）。以下是正确与错误使用引号及属性/值对的示例。

正确示例：

```html
<input type="text" name="email" disabled="disabled" />
<input type='text' name='email' disabled='disabled' />
```

错误示例：

```html
<input type=text name=email disabled>
```

在 HTML 中，并非所有属性都必须有值，属性值也不总是需要引号包裹。虽然上述所有示例在 HTML 中都是有效的，但**属性不加引号可能导致安全漏洞**。请始终为属性添加引号。布尔属性允许省略值，但值 `true` 和 `false` 对布尔属性无效（[HTML5 规范来源](https://www.w3.org/TR/2011/WD-html5-20110405/common-microsyntaxes.html#boolean-attributes)）。

正确示例：

```html
<input type="text" name="email" disabled />
```

错误示例：

```html
<input type="text" name="email" disabled="true" />
```

### 缩进

与 PHP 类似，HTML 缩进应始终反映逻辑结构。使用制表符而非空格。

当混合使用 PHP 和 HTML 时，PHP 代码块的缩进应与周围的 HTML 代码保持一致。结束的 PHP 代码块应与起始代码块保持相同的缩进级别。

正确示例：

```php
<?php if ( ! have_posts() ) : ?>
<div id="post-1" class="post">
	<h1 class="entry-title">Not Found</h1>
	<div class="entry-content">
		<p>Apologies, but no results were found.</p>
		<?php get_search_form(); ?>
	</div>
</div>
<?php endif; ?>
```

错误示例：

```php
<?php if ( ! have_posts() ) : ?>
<div id="post-0" class="post error404 not-found">
<h1 class="entry-title">Not Found</h1>
<div class="entry-content">
<p>Apologies, but no results were found.</p>
<?php get_search_form(); ?>
</div>
</div>
<?php endif; ?>
```

## 鸣谢

- HTML 代码标准改编自 [Fellowship Tech 代码标准](https://developer.fellowshipone.com/patterns/code.php) ([CC 许可协议](https://creativecommons.org/licenses/by-nc-sa/3.0/))。