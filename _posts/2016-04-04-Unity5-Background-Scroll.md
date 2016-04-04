---
layout: post
title: unity5 게임 배경 이동
---

Background-Image-Scroll-on-Unity

유니티5로 게임 배경화면 이동을 구현했다.ㅅㄷㄴㅅ

![구성](/images/comp.PNG)

위와 같이 3가지 오브젝트를 사용했다.

![camera](/images/maincamera.PNG)

>>Main Camera's Inspector view

![player](/images/player.PNG)

_-Player's Inspector view_

![background](/images/background.PNG)

_-Background's Inspector view_

![playerimage](/images/ship.PNG)

_-Player 오브젝트의 이미지 설정_

![backgroundimage](/images/backmaterial.PNG)

_-Background 오브젝트의 Material 설정_

Background 오브젝트에 있는 스크립트의 내용은 아래와 같다.

```c#
using UnityEngine;
using System.Collections;

public class BackScroll : MonoBehaviour {

    public float scrollSpeed = 1.5f;
    Material myMaterial;

	// Use this for initialization
	void Start () {
        myMaterial = GetComponent<Renderer>().material;
	}
	
	// Update is called once per frame
	void Update () {
        float newOffsetY = myMaterial.mainTextureOffset.y + scrollSpeed * Time.deltaTime;
        Vector2 newOffset = new Vector2(0, newOffsetY);
        myMaterial.mainTextureOffset = newOffset;
	}
}
```

이렇게 하고 실행하면 배경이 움직인다.
