---
layout: post
title: unity5 게임 배경 이동
---

Background-Image-Scroll-on-Unity

유니티5로 게임 배경화면 이동을 구현했다.

![구성](/images/comp.png)

위와 같이 3가지 오브젝트를 사용했다.

실질적으로 사용한 것은 카메라를 제외한 Player와 Background 오브젝트다.

![player](/images/player.png)

>Player's Inspector view

![background](/images/background.png)

>Background's Inspector view

Background 오브젝트에 있는 스크립트의 내용은 아래와 같다.

```C#

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

이렇게 하고 실행하면 배경 움직인다.
