---
layout: post
title: unity5 코루틴
---

이번엔 코루틴을 사용해서 게임이 시작하기 전에 Ready라는 텍스트가 3번 깜빡이고

시작하게 만들 것이다.

우선 코루틴이란

1. 코루틴( co-routine)함수

(1) 사용예 : 적이 근처에 있을 때 player에게 알려주는 기능을 구현할 때, 매 프레임마다 호출하면 오버해드 초래함. 일정시간(1/10초) 간격으로 호출하여 게임의 악영향을 감소 시키고, 체크 횟수를 감소시킬 수 있다.

(2) 코루틴함수를 사용할 경우 : 
     1) 어떤 루틴이 한단계씩 호출되어야 할 경우
     2) 어떤 루틴이 일정시간을 두고 호출되어야 할 경우
     3) 어떤 루틴이 다른 작업이 완료될때 까지 대기 해야 하는 경우

(3) 코루틴 함수의 기능: 코루틴은 실행을 중지하여 unity에 제어권을 넘겨주고, 계속 할때는 중지한 곳부터 계속 실행할 수 있는 기능임

(4) 코루틴 함수의 리턴타입은 IEnumerator 이다.
    yeild return 구문을 갖는다.
    호출할때 StartCoroutine(함수명); 형태로 호출한다.

우선 game 씬으로 이동해서

Text 오브젝트를 만들고 자신이 원하는 대로 설정을 하고

GameTextControl이라는 cs스크립트를 만들어서 Canvas에 연결해준다.

그리고 아래 소스를 적어준다.

```c#
using UnityEngine;
using System.Collections;

public class GameTextControl : MonoBehaviour {

    public GameObject readText;
    public static GameTextControl instance;


    void Awake()
    {
        if (GameTextControl.instance == null)
            GameTextControl.instance = this;
    }

    // Use this for initialization
    void Start()
    {
        readText.SetActive(false);
        StartCoroutine(ShowReady());
    }

    IEnumerator ShowReady()
    {
        int count = 0;
        while (count < 3)
        {
            readText.SetActive(true);
            yield return new WaitForSeconds(0.5f);
            readText.SetActive(false);
            yield return new WaitForSeconds(0.5f);
            count++;
        }
    }

    // Update is called once per frame
    void Update()
    {

    }

}
```

그리고 아까 연결한 스크립트에 아까 만든 Text 오브젝트를 연결해주면 끝난다.
