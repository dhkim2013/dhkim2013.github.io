---
layout: post
title: Execute-the-explosion-animation-when-the-objects-collide
---

unity5로 오브젝트가 충돌하면 폭발하는 애니메이션이 재생되도록 구현했다.

![1](/images/04151.PNG)

위 사진과 같이 폭발 애니메이션을 만든다.

그 후 애니메이션을 재생하면 애니메이션이 무한루프를 돈다.

무한루프를 없애기 위해서

![3](/images/04153.PNG)

위의 사진과 같은 애니메이션 파일의 Loop Time의 체크를 아래 사진과 같이 해제한다.

![2](/images/04152.PNG)

무한루프를 해제하고 실행을 해도 마지막에 잔상이 남는다 그 잔상을 없애기 위해 스크립트 하나를 만들고 아래와 같이 소스를 적고 폭발 오브젝트에 연결해준다.

```c#
using UnityEngine;
using System.Collections;

public class explosion : MonoBehaviour {

	// Use this for initialization
	void Start () {
        Destroy(this.gameObject, 0.8f);
	}
	
	// Update is called once per frame
	void Update () {
	
	}
}
```

위의 스크립트를 연결한 후에 실행하면 폭발효과가 0.8초 뒤에 사라진다.

이제 프리팹이라는 것을 만들어 볼 것이다.

프리팹이란 폭발 효과와 같은 필요할 때 호출하는 게임오브젝트의 한 종류이다.

프리팹을 사용하지 않으면 메모리의 낭비가 되기 때문에 간결하고 재사용이 편리한 프리팹을 사용한다.

아까 Hierarchy view에 만든 explosion 애니메이션을 

![4](/images/04157.PNG)

위와 같이 Project view에 드래그 하면 해당 애니메이션의 프리팹이 만들어진다.

이 프리팹을 이용하여 객체가 충돌하면 폭발효과가 생긴 후 사라지게 만들겠다.

이전에 만들었던 player 스크립트에 아래와 같이 소스를 몇줄 넣어주면 완성된다.

```c#
using UnityEngine;
using System.Collections;

public class player : MonoBehaviour {

    public float moveSpeed = 0.5f;
    public GameObject explosionPrefab;

	// Use this for initialization
	void Start () {
	
	}

	// Update is called once per frame
	void Update () {
        MoveControl();
	}

    void MoveControl()
    {
        float moveX = moveSpeed * Time.deltaTime * Input.GetAxis("Horizontal");
        transform.Translate(moveX, 0, 0);

    }

    void OnTriggerEnter2D(Collider2D col)
    {
        if (col.gameObject.tag == "Enemy")
        {
            Instantiate(explosionPrefab, transform.position, Quaternion.identity);
            Destroy(col.gameObject);
            Destroy(this.gameObject);
        }
    }

}
```
