---
title: "[BASH] getopts"
author: yoonkij
date: 2022-10-25 09:00:00 +0800
categories: [Dev, Bash]
tags: [bash]
render_with_liquid: true
---

업무상 여러 데이터를 처리하는 모듈을 개발을 하며 spark-job을 다수, 연속적으로 호출하게 된다. 

여러 단계의 처리를 위해 spark-submit을 비슷한 옵션과 함께 여러군데에서 사용하게 되는데, 이렇게 같은 코드가 반복되는 경우 스크립트와 옵션으로 묶어 간단하면서도 편리하게 활용하는 편이다.

이 글에서는 `python의 argparse`와 같이 bash script 상에서 command argument를 파싱해서 사용할 수 있게 해주는 `getopts`에 대해 알아본다.

### Positional Parameters

`positional parameters` 는 스크립트 혹은 함수에 전달되는 인자가 순서대로 설정된 로컬 파라미터로 $1, $2, $3 … 과 같이 순서대로 설정된다. (두자리 이상의 경우 ${11}과 같이 표현해야한다)

`set`명령으로 shell 옵션 on/off 외에도 임의의 positional parameters를 설정할 수있다. 

설정하고자 하는 argument에 '-'가 포함되어있을 경우 `--`를 추가하여 그 뒤가 모두 parameter로 인식되게 할 수 있다. `set —-help` 를 통해서도 해당 내용을 확인할 수 있다.

**`set --help`  에서 발췌**
```bash
$ set --help
  set: set [-abefhkmnptuvxBCHP] [-o option-name] [--] [arg ...]
  Set or unset values of shell options and positional parameters.
 
  ... (생략)
			
    --  Assign any remaining arguments to the positional parameters.
        If there are no remaining arguments, the positional parameters
        are unset.
    -   Assign any remaining arguments to the positional parameters.
        The -x and -v options are turned off.
  ```
    
**예시**
```bash
# set 으로 임의 설정 가능
$ set arg1 arg2 arg3 arg4 arg5
$ echo $1, $2, $3
>> arg1, arg2, arg3

# -a가 set의 argument로 인식되고 arg1부터 parameter로 인식
$ set -a arg1 -b arg2 -c arg3
$ echo $1, $2, $3
>> arg1 -b arg2

# -- 를 추가하여 뒤가 모두 parameter로 인식
$ set -- -a arg1 -b arg2 -c arg3
$ echo $1, $2, $3
>> -a arg1 -b
```

### Special Parameters

bash shell은 몇 가지 특수 파라미터를 가진다. 그 중 positional parameters와 관련된 몇가지는 아래와 같다.

- `$0` : 현재 스크립트 파일의 이름
- `$#` : positional parameters의 수
- `$@` : 모든 positional parameters를 space로 구분한 word
    - 이와 유사한 기능을 하는 `$*` 도 있지만 자세한 차이는 생략

### getopts

```bash
getopts optstring optname [ arg ]
```

`getopts` 는 주어진 옵션에 따라 positional argument를 파싱하여 한번에 하나씩 돌려준다. 따라서, while 등의 loop와 함께 입력 처리하면 쉽다.

(디테일하게는 `OPTIDX` 변수와 함께 다뤄야 하나 일단 생략)

`optstring`은 파싱할 옵션을 명시하는 문자열을 말한다. 특징으로는

- single character option을 지원한다. -a -c … (multiple/long option은 다른 방식을 활용해야 한다)
- on/off 스위치가 아닌 argument value를 동반하는 경우 option character뒤에 “:”를 붙인다.
- `optstring` 맨 앞에 “:”를 붙일 경우, silent error checking 옵션이 켜져 에러가 표시되지 않으며, 별도 예외 처리를 직접 해야한다.

예를 들어 `-a optionA -c optionC -d` 와 같은 옵션을 활용하고 싶다면, optstring에 `a:c:d` 를 넣으면 된다. (verbose error checking)

`optname` 은 위에서 설정한 single character를 받을 변수명이다.

흔히 활용하는 조합은 getopts + while + switch 이다.

```bash
#!/bin/bash
# arg_test.sh

A_OPT=""
B_OPT=""
while getopts ":a:b:c" opt; do
	case $opt in
		a) A_OPT=$OPTARG; echo "option -a = $A_OPT" ;; # a 옵션인경우 처리
		b) B_OPT=$OPTARG; echo "option -b = $B_OPT" ;; # b 옵션인경우 처리
		c) echo "option -c called" ;; # c 옵션인경우 처리
		:) value가 필요하지만 주어지지 않은 경우 처리 ;;
		*) 알 수 없는 옵션 처리 ;;
	esac
done
```

while문을 돌면서

1. silient 모드로 
2. argument가 필요한 a, b + 필요 없는 c 옵션을 파싱하여 
3. argument name은 `opt` 에, value는 `OPTARG` 에 받아
4. case문으로 각 옵션에 따라 처리한다.
5. value 없거나 정의되지 않은 argument 예외처리도 잊지말자.

```bash
$ sh arg_test.sh -a 11 -b 22 -c
option -a = 11
option -b = 22
option -c called

$ sh arg_test.sh -b 22 -a 11
option -b = 22
option -a = 11
```

### Reference

- [https://mug896.github.io/bash-shell/getopts.html](https://mug896.github.io/bash-shell/getopts.html)
- [https://mug896.github.io/bash-shell/positional_parameters.html](https://mug896.github.io/bash-shell/positional_parameters.html)
- [https://www.computerhope.com/unix/bash/getopts.htm](https://www.computerhope.com/unix/bash/getopts.htm)