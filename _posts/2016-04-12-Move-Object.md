---
layout: post
title: unity5 충돌처리
---

Collision-Object

이번에는 unity5로 오브젝트의 충돌처리를 구현했다.

나는 Player 객체와 Enemy 객체가 충돌을 하면 두 객체가 서로 사라지도록 구현을 해봤다.

![1](/images/04112-1.PNG)

_-Player's Inspector view_

이전에 만들었던 Player 객체에 Circle Collider 2D와 RigidBody 2D가 생겼다.

Player 객체를 클릭한 후에 메뉴에 Component - Physics 2D - Circle Collider 2D를 클릭하면 Circle Collider 2D가 생긴다.

그리고 Player 객체를 클릭한 후에 Component - Physics 2D - Rigidbody 2D를 클릭하면 Rigidbody 2D가 생긴다.

두 컴포넌트를 추가한 후에 Circle Collider 2D의 Is Trigger에 체크를 한다.

Is Trigger를 체크하는 이유는 충돌을 한 후에 아무 처리를 하지 말라는 의미로 체크를 하는 것이다.

그리고 Rigidbody 2D의 Is Kinematic에 체크를 한다.

Is Kinematic은 중력의 영향을 받지 말라고 체크를 하는 것이다.

Player 객체의 설정을 마쳤으니 이제 같은 방법으로 Enemy 객체에 두 컴포넌트를 추가했다.

![2](/images/04112-2.PNG)

Enemy는 Circle Collidder 2D가 아닌 Box Collider 2D를 했다.

두 컴포넌트는 충돌판정을 받는 모양만 다르다.

설정을 마쳤으면 기존 Player 객체에 연결되어 있는 스크립트를 아래와 같이 수정하면 끝난다.

```c#
using UnityEngine;
using System.Collections;

public class Player : MonoBehaviour {

    public float moveSpeed = 0.5f;

    void moveControl()
    {
        float moveX = moveSpeed * Time.deltaTime * Input.GetAxis("Horizontal");
        transform.Translate(moveX, 0, 0);
    }

    void OnTriggerEnter2D(Collider2D col)
    {
        if (col.gameObject.tag == "Enemy")
        {
            Destroy(col.gameObject);
            Destroy(this.gameObject);
        }
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
