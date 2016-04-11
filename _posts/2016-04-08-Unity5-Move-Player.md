---
layout : post
title : unity5 player 이동
---

Move-Player

unity5 플레이어 캐릭터를 좌우로 움직이는 것을 구현했다.

일단은 캐릭터를 키입력이 없이 움직이게 소스를 짜봤다.

![1](/images/040801.PNG)

_-Player's Inspector view_

우선 위와 같이 Player라는 이름을 가진 스크립트를 Player 오브젝트에 추가시켰다.

```c#
using UnityEngine;
using System.Collections;

public class Player : MonoBehaviour {

    public float moveSpeed = 0.5f;

    void moveControl() {
        float moveX = moveSpeed * Time.deltaTime;
        transform.Translate(moveX, 0, 0);
    }

	// Use this for initialization
	void Start () {
	    
	}
	
	// Update is called once per frame
	void Update () {
        moveControl();
	}
}
```

그 다음 Player 스크립트에 위와 같은 소스를 입력해준다.

소스를 저장한 후에 유니티를 실행하면 Player 오브젝트가 오른쪽으로만 움직일 것이다.

이제 위의 기능에 InputManager를 이용해 키입력을 통해 Player 오브젝트를 움직일 것이다.

![2](/images/040802.PNG)

_-InputManager_

위 사진은 InputManager의 사진이다.

InputManager는 유니티의 메뉴에 Edit - Project Settings - Input에서 볼 수 있다.

위 사진을 보면 Negative Button과 Positive Button이 보일 것이다.

Negative Button에 해당된 키를 입력하면 -1을 반환하고,

Positive Button에 해당된 키를 입력하면 1을 반환한다.

```c#
using UnityEngine;
using System.Collections;

public class Player : MonoBehaviour {

    public float moveSpeed = 0.5f;

    void moveControl() {
        float moveX = moveSpeed * Time.deltaTime * Input.GetAxis("Horizontal");
        transform.Translate(moveX, 0, 0);
    }

	// Use this for initialization
	void Start () {
	    
	}
	
	// Update is called once per frame
	void Update () {
        moveControl();
	}
}
```

InputManager를 이용해 Player 오브젝트를 왼쪽, 오른쪽 방향키로 원하는 만큼 좌우로 움직일 수 있게 되었다.
