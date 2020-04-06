# Aizu Online Judge how to solve ITP-1

## ITP1_1_A  Hellow World

print() 関数を使う

## ITP1_1_B X cubic

x = int(input()) とすることで入力された文字を数字にできる

## ITP1_1_C Rectangle

一行に２つの数字が入ってくる

### Sample 1

```
s = input()
x = s.split()
a = int(x[0])
b = int(x[1])
```

### Sample 2

```
a,b = map(int, input().split())
```

## ITP1_1_D Watch

1min は 60sec, 1hour は 60min(3600sec), せいすうのわりざん (//) とあまり (%) をつかう。
アウトプットは print かんすうの sep オプションをつかうか、もじれつの format メソッドをつかう
