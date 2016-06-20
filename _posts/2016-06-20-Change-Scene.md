---
layout: post
title: unity5 씬 전환
---

Change-Scene

오늘은 씬을 전환할 것이다.

Save Scene as로 씬을 새로 저장하고

SceneManager라는 이름의 빈 오브젝트를 만들어 준다.

그리고 SceneManger라는 이름의 cs 스크립트를 만들고

```c#
using UnityEngine;
using System.Collections;

public class SceneManager : MonoBehaviour {

	// Use this for initialization
	void Start () {
	
	}
	
	// Update is called once per frame
	void Update () {
	
	}

    public void ChangeToScene()
    {
        Application.LoadLevel("game");
    }
}
```
위와 같이 소스를 적어준다.

Application.LoadLevel() 함수는 Scene을 바꿔주는 함수다.

매개변수에 바꿀 Scene의 이름을 적어주면 된다.

위와 같이 작성하면 game이라는 이름의 Scene으로 전환된다.

GameObject - UI - Button을 통해서 버튼 오브젝트를 만들어 준다.

버튼 오브젝트의 Inspector view에 On Click () 부분에 None에

SceneManager 오브젝트를 연결해주고 오른쪽 위에 있는 드롭다운에서 SceneManager - ChangeToScene을 눌러주면 된다.
