---
title: "无障碍编码标准"
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

# 无障碍编码标准

集成到 WordPress 生态系统的代码——包括 WordPress 核心、WordPress.org 网站和官方插件，都应遵循《Web 内容无障碍指南》(WCAG) 2.2 版的 AA 级别标准。

鼓励新的或更新的界面融入《创作工具无障碍指南》(ATAG) 2.0。融入 ATAG 2.0 指南最重要的方式是强调有助于人们创建更无障碍内容的选择：例如，鼓励使用替代文本、字幕和语义结构。

关于网络无障碍标准的官方信息可分为两类："规范性"和"参考性"文档。只有指南本身是规范性的，并确立了遵循 WCAG 2.2 的实际要求。其他文档应被视为参考性信息，为解读指南提供帮助，但不具决定性。

WordPress A11y 团队正在开发一个推荐的无障碍模式库，以帮助描述 WordPress 推荐的各种界面实现方式。这些可能不是创建无障碍模式示例的唯一合理方式，但为了保持 WordPress 生态系统的一致性，它们是首选方法。

规范性文档：

- [W3C WCAG 2.2](https://www.w3.org/TR/WCAG22)
- [W3C ATAG 2.0](https://www.w3.org/TR/ATAG20/)
- [W3C WAI ARIA 1.1](https://www.w3.org/TR/wai-aria/)

参考性文档：

- [W3C 理解 WCAG 2.2](https://www.w3.org/WAI/WCAG22/Understanding/)
- [W3C 使用 ARIA](https://www.w3.org/TR/using-aria/)
- [W3C WAI-ARIA 创作实践指南（无障碍设计模式）](https://www.w3.org/WAI/ARIA/apg/)
- [W3C ATAG 简介](https://www.w3.org/WAI/standards-guidelines/atag)

## 关于 WCAG A、AA 和 AAA 一致性级别

WordPress 致力于遵循所有 WCAG 2.2 A 级和 AA 级指南。在相关情况下，鼓励达到 AAA 级成功标准，并超越这些指南的可访问性要求。

**A 级** 成功标准解决的是被认为在非常广泛的范围内构成可访问性障碍的问题，这些问题会阻止许多人访问网站，并且是大多数基于 Web 的界面所需实现的最低目标集。

**AA 级** 成功标准解决的是通常解决起来稍微复杂一些、可能影响较小群体但仍然具有广泛影响的常见需求。

**AAA 级** 成功标准主要针对非常具体的需求，可能很难有效实施。

[W3C WCAG 2.2 A 级和 AA 级要求快速参考](https://www.w3.org/WAI/WCAG22/quickref/?versions=2.2&currentsidebar=%23col_customize&levels=aaa)

## 应用 WCAG 符合性等级

WCAG 2.2 包含 4 个层次：

- 原则
- 指南
- 成功标准
- 充分和咨询性技术

### 原则

在应用 WCAG 2.2 时，指南和成功标准围绕 4 个原则组织。这些原则强调人们如何与内容互动，并且必须满足：

- **可感知** - 使用他们熟悉的媒介与内容互动。例如，为盲人提供文本替代方案。
- **可操作** - 查找和使用内容是可达的。例如，能够使用键盘或屏幕阅读器。
- **可理解** - 内容使用清晰的语言且易于理解。例如，使用有意义的标签，解释所有缩写。
- **健壮** - 内容可以通过多种方式解释。例如，辅助技术能够解释和解析内容。

### 指导原则

每项原则都附有一系列指导准则，以确保内容在不同设备上更易于访问和呈现，满足用户的残障需求。指导准则如下，完整细节请参阅 WCAG 2.2。

#### 原则：可感知性

**准则 1.1 文本替代方案**
为非文本内容提供文本替代方案，以便其可以转换为人们所需的其他形式，例如大字体、盲文、语音、符号或更简单的语言。

**准则 1.2 基于时间的媒体**
为基于时间的媒体提供替代方案。例如，为音频或视频片段包含字幕和文字记录。

**准则 1.3 适应性**
创建能够以不同方式（例如更简单的布局）呈现的内容，且不丢失信息或结构。

**准则 1.4 可区分性**
使用户更容易看到和听到内容，包括将前景与背景分离。

#### 原则：可操作性

**准则 2.1 键盘可访问**
确保所有功能均可通过键盘操作。

**准则 2.2 充足时间**
为用户提供足够的时间来阅读和使用内容。

**准则 2.3 癫痫与生理反应**
避免设计已知会引发癫痫或生理反应的内容。

**准则 2.4 可导航性**
提供帮助用户导航、查找内容及定位自身位置的方法。

**准则 2.5 输入方式**
让用户能通过键盘以外的多种输入方式更便捷地操作功能。

#### 原则：可理解性

**准则 3.1 可读性**
使文本内容易于阅读和理解。

**准则 3.2 可预测性**
使网页以可预测的方式呈现和操作。

**准则 3.3 输入辅助**
帮助用户避免和纠正错误。

#### 原则：健壮性

**指南 4.1 兼容性**
最大限度地兼容当前和未来的用户代理，包括辅助技术。

### 成功标准

每条指南都有一份[具体的要求清单，您的内容必须满足这些要求才能实现无障碍访问](https://www.w3.org/WAI/WCAG21/quickref/)。这些测试可以通过自动化软件和/或人工测试员来执行。您可以在[理解一致性级别](https://www.w3.org/WAI/WCAG21/Understanding/conformance#levels)中找到关于如何满足成功标准的更多信息。虽然这些标准很重要，但可用性测试仍然至关重要，应与任何无障碍测试一同进行。

### 技巧：充分性、建议性与失败案例

用于指导原则和成功标准的技巧（代码示例、资源和测试）分为三类，有助于提升内容的可访问性：

- 充分性技巧 - 必需且有助于满足成功标准
- 建议性技巧 - 超出必需范围的建议
- 失败案例 - 导致问题且未能满足成功标准

如需了解更多技巧信息，请访问[理解 WCAG 成功标准技巧](https://www.w3.org/WAI/WCAG21/Understanding/understanding-techniques)。

## Authoritative Resources

- [WebAIM: Web Accessibility In Mind](https://webaim.org/) (see Articles and Resources)
- [UK Government Digital Service](https://gds.blog.gov.uk)
- [Accessibility in Government Blog (UK)](https://accessibility.blog.gov.uk/)
- [Create Accessible Software & Websites - Section 508 (US)](https://www.section508.gov/develop/software-websites/)
- [Blog | TPGi](https://www.tpgi.com/blog/)
- [Web Accessibility Blog (Deque)](https://www.deque.com/blog/)
- [Tink - Léonie Watson](https://tink.uk) (Léonie Watson)
- [Adrian Roselli](https://adrianroselli.com)
- [Scott O'Hara](https://www.scottohara.me)
- [Joe Dolson](https://www.joedolson.com/blog)
- [Sarah Higley](https://sarahmhigley.com/)
- [Marco's Accessibility Blog](https://www.marcozehe.de/)
- [Karl Groves](https://karlgroves.com/)
- [Inclusive Components](https://inclusive-components.design) (Heydon Pickering)
- [Accessibility London (London, United Kingdom)](https://www.meetup.com/London-Accessibility-Meetup/) (London accessibility meetup: they live stream meetups on youtube)
- [24 Accessibility](https://www.24a11y.com/)
- [Mozilla Accessibility - Users first, no matter their abilities](https://blog.mozilla.org/accessibility/)
- [WordPress Accessibility Meetup](https://www.meetup.com/wordpress-accessibility-meetup-group/)
- [Equalize Digital Blog](https://equalizedigital.com/resources/)
- [WordPress Accessibility Day Conference](https://wpaccessibility.day)

### Technical and / or specific topics

- [Accessibility Support](https://a11ysupport.io/) (Will your code work with assistive technologies?)
- [Accessibility APIs: A Key To Web Accessibility](https://www.smashingmagazine.com/2015/03/web-accessibility-with-accessibility-api/) (by Léonie Watson)
- [How accessibility trees inform assistive tech](https://hacks.mozilla.org/2019/06/how-accessibility-trees-inform-assistive-tech/) (by Hidde de Vries)
- [What is this thing and what does it do?](https://www.youtube.com/watch?v=YLihNhn_MO4 ) (presentation by Karl Groves)
- [The Browser Accessibility Tree](https://www.tpgi.com/the-browser-accessibility-tree/) (by Steve Faulkner)
- [Brief history of browser accessibility support](https://www.tpgi.com/brief-history-of-browser-accessibility-support/) (by Steve Faulkner)
- [ARIA Landmarks Example: General Principles](https://www.w3.org/TR/wai-aria-practices/examples/landmarks/)
- [ARIA Landmarks Example: HTML Sectioning Elements](https://www.w3.org/TR/wai-aria-practices/examples/landmarks/HTML5.html)
- [Mozilla Developer Docs - Accessibility](https://developer.mozilla.org/en-US/docs/Web/Accessibility)