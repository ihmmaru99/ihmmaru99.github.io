---
title: "[프로그래머스] 셔틀버스"
categories:
  - Programmers
tags:
  - [Algorithm, C++, Implementation]
date: 2023-06-08
last_modified_at: 2023-06-09
---
# 2018 KAKAO BLIND RECRUITMENT 셔틀버스
## 문제
카카오에서는 무료 셔틀버스를 운행하기 때문에 판교역에서 편하게 사무실로 올 수 있다. 카카오의 직원은 서로를 '크루'라고 부르는데, 아침마다 많은 크루들이 이 셔틀을 이용하여 출근한다.
이 문제에서는 편의를 위해 셔틀은 다음과 같은 규칙으로 운행한다고 가정하자.
- 셔틀은 09:00부터 총 n회 t분 간격으로 역에 도착하며, 하나의 셔틀에는 최대 m명의 승객이 탈 수 있다.
- 셔틀은 도착했을 때 도착한 순간에 대기열에 선 크루까지 포함해서 대기 순서대로 태우고 바로 출발한다. 예를 들어 09:00에 도착한 셔틀은 자리가 있다면 09:00에 줄을 선 크루도 탈 수 있다.

일찍 나와서 셔틀을 기다리는 것이 귀찮았던 콘은, 일주일간의 집요한 관찰 끝에 어떤 크루가 몇 시에 셔틀 대기열에 도착하는지 알아냈다. 콘이 셔틀을 타고 사무실로 갈 수 있는 도착 시각 중 제일 늦은 시각을 구하여라.
단, 콘은 게으르기 때문에 같은 시각에 도착한 크루 중 대기열에서 제일 뒤에 선다. 또한, 모든 크루는 잠을 자야 하므로 23:59에 집에 돌아간다. 따라서 어떤 크루도 다음날 셔틀을 타는 일은 없다.

## 입력 형식
셔틀 운행 횟수 n, 셔틀 운행 간격 t, 한 셔틀에 탈 수 있는 최대 크루 수 m, 크루가 대기열에 도착하는 시각을 모은 배열 timetable이 입력으로 주어진다.
- 0 ＜ n ≦ 10
- 0 ＜ t ≦ 60
- 0 ＜ m ≦ 45
- timetable은 최소 길이 1이고 최대 길이 2000인 배열로, 하루 동안 크루가 대기열에 도착하는 시각이 HH:MM 형식으로 이루어져 있다.
- 크루의 도착 시각 HH:MM은 00:01에서 23:59 사이이다.

## 출력 형식
콘이 무사히 셔틀을 타고 사무실로 갈 수 있는 제일 늦은 도착 시각을 출력한다. 도착 시각은 HH:MM 형식이며, 00:00에서 23:59 사이의 값이 될 수 있다.

## 풀이
> 문제에서 사람이 오는 시간은 배열에 문자열의 형태로 저장되어 있다. 문자열을 정수로 반환하여 도착한 시간을 확인할 수 있도록 한다. 정수로 변환된 사람들의 도착시간을 오름차순으로 정렬한다. 정렬된 사람들을 통해서 셔틀버스에 해당 시간에 탈 수 있는지 확인한다. 만약 셔틀버스에 탈 수 있는 잔여석이 남아있고 현재 사람이 도착해있다면 그 사람은 해당 셔틀버스에 탈 수 있고 탔음을 표시한다. 그렇게 사람을 다 넣었으면 콘이 셔틀버스에 탈 수 있는 시간을 확인해야 한다. 만약 현재 도착한 사람들을 모두 태웠을 때 내가 탈 수 없다면 마지막에 탄 사람보다 1분 빨리 와야 하고 아니고 아직 맨 마지막에 정확히 도착하는 셔틀버스를 타도록 하면 된다. 이렇게 저장된 콘이 도착해야 되는 시간을 다시 문자열로 변환해서 출력할 수 있도록 하면 된다.

## 결과
[셔틀버스](https://github.com/ihmmaru99/Programmers/blob/main/셔틀버스/셔틀버스.cpp)
```c++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

string solution(int n, int t, int m, vector<string> timetable) {
    int result;
    string answer = "";
    vector<int> v;
    for(string time : timetable)
        v.push_back(stoi(time.substr(0,2)) * 60 + stoi(time.substr(3,2)));
    sort(v.begin(), v.end());
    int bus = 540;
    int index = 0;
    for(int i=0; i<n; i++){
        int cnt = 0;
        for(int j=index; j<v.size(); j++){
            if(v[j] <= bus){
                index++;
                cnt++;
                if(cnt == m)
                    break;
            }
        }
        if(i == n - 1){
            if(cnt == m){
                result = v[index-1]-1;
            }
            else
                result = bus;
        }
        bus += t;
    }
    int h = result / 60;
    if(h < 10)
        answer += "0" + to_string(h) + ":";
    else
        answer += to_string(h) + ":";
    int minutes = result % 60;
    if(minutes < 10)
        answer += "0" + to_string(minutes);
    else
        answer += to_string(minutes);
    return answer;
}
```

## 채점 결과
<img width="149" alt="스크린샷 2023-06-09 12 07 33" src="https://github.com/ihmmaru99/Programmers/assets/109266664/0ae294f6-be8e-4f23-9afb-2b8e1d5d33b4">
