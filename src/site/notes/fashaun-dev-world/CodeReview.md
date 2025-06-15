---
{"dg-publish":true,"permalink":"/fashaun-dev-world/code-review/","noteIcon":""}
---

# Code Review Do Not Find Bugs
- microsoft.com/en-us/research/wp-content/uploads/2015/05/PID3556473.pdf
# Auto Code Review
- 程式碼寫法檢查
	- 可維護性
	- 安全性
	- 擴展性

# Peer Code Review
- 人爲觀點
- 相容性檢查
- 順手撿垃圾


# General Rule 

## Regular Rule 
> 基本的 review SOP
## Nit 
> 額外有興趣看的

# Comment Abbr/Style
- **Nit: Personal Preference and Non-Essential Changes**

There are often many different ways to implement the same functionality. Unless the code has issues related to functionality, readability, consistency, or optimization—as mentioned in the previous section—most differences come down to personal preference and are not strictly necessary to change.

In such cases, you can add "Nit" in your code review comments. "Nit" stands for "nitpick," which means pointing out minor, non-critical issues. In other words, it suggests: _"This is a small issue—nice to fix if possible, but acceptable to leave as is."

**Focus on the Code, Not the Person**

Avoid making comments that target the individual rather than the code. For example, saying _"You didn’t check for null values"_ places the focus on _“you”_. Instead, rephrase the comment to focus purely on the code, such as: _"This input might be null, which could lead to an error. It’s recommended to add a null check here."_

Alternatively, you can ask guiding questions to help the author think through potential issues, like: _"Could this value be null? If so, might that cause a problem?"_
# Smallest Software Component Review
- Smallest Test Unit

# Ref