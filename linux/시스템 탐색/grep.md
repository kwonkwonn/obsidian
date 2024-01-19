

## 기본 사용법 

	grep [옵션] 패턴 [파일]

``` 
grep -i 'uuid' /etc/fstab
// -i <- 대소문자 가리지 말고 /etc/fstab에서 uuid와 일치하는 패턴 찾기
```

	grep [옵션] [-e 패턴 | -f 파일] [파일 ] 

```
grep -i -e "^\[[[:alnum:]]*\]" /etc/nova/nova.conf
//대괄호가 []가 앞뒤에 있는 문자열 검색

grep -i -e "^\[[[:alnum:]]*\]" -e "^virt_type" \ /etc/nova/nova.conf
//대괄호가 []가 앞뒤에 있거나 virt_type으로 시작하는 문자열 검색 

grep -i -f pattern1.txt /etc/nova/nova.conf
//pattern1.txt파일안에 패턴이 저장되어 있을 경우 사용가능

```

	명령어 | grep [옵션] [패턴| -e 패턴 ]

``` cat /etc/nova/nova.conf | grep -i '^\[Default' [DEFAULT]
// cat을 이용하여 확인한 /etc/nova/nova.conf파일내용이 grep의 검색 대상이 됨.
// [Defaul 로 시작하는 문장을 해당 파일에서 찾음
```


## 다양한 옵션들

