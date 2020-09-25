---
title: "minishell"
excerpt: "42Seoul minishell Guide documents"
date: 2020-09-25 13:56:42 +0900
categories: 42Seoul
---

bash와 동일한 동작을 하는 shell을 구현

## builtin function

- echo
	- option -n을 포함
- cd
	- no option, relative or absolute path
- pwd
	- no option
- export
	- no option
- unset
	- no option
- env
	- no option, no arguments
- exit
	- no option

## 구현해야 할 사항

- ; 동작 bash와 동일하게
- ', " 동작 구현, multiline commands는 제외
- <, >, >> redirection 구현, file descriptor aggregation 제외
- | (pipeline)은 bash와 같이 동작
- 환경변수 역시 bash와 마찬가지로 동작
- $?
- ctrl+C, ctrl+D, ctrl+\ 역시 bash와 같은 결과