---
layout: post
title: unity5 레이저 발사
---

Shoot-Laser

이번에는 플레이어가 레이저를 발사하는 것을 구현했다.

이전에 해봤던건 재차 설명을 안할거다.

일단 Laser라는 이름의 애니메이션을 프리팝으로 만들고

![1](/images/04181-1.PNG)

위 사진과 같이 Laser라는 이름의 태그를 붙여주고, Box Collider 2D와 Rigidbody 2D라는 컴포넌트를 붙여줬다.

이전과 같이 Is Trigger와 Is Kinematic의 체크를 해줬다.

그 다음에 아래 스크립트를 Laser 프리팝에 연결해준다.

```c#
using UnityEngine;
using System.Collections;

public class Laser : MonoBehaviour {

    public float moveSpeed = 0.45f;

	// Use this for initialization
	void Start () {
	
	}
	
	// Update is called once per frame
	void Update () {
        float moveY = moveSpeed * Time.deltaTime;
        transform.Translate(0, moveY, 0);
	}
}
```

이제 Player 오브젝트를 설정해보자

이전에 작성했던 Player 스크립트에 아래와 같이 수정한다.

```c#
using UnityEngine;
using System.Collections;

public class Player : MonoBehaviour
{

    public float moveSpeed = 0.5f;
    public GameObject explosionPrefab;
    public GameObject laserPrefab;
    public bool canShoot = true;
    float shootDelay = 0.5f;
    float shootTimer = 0;

    // Use this for initialization
    void Start()
    {

    }

    // Update is called once per frame
    void Update()
    {
        MoveControl();
        ShootControl();
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

    void ShootControl()
    {
        if (canShoot)
        {

            if (shootTimer > shootDelay)
            {
                Instantiate(laserPrefab, transform.position, Quaternion.identity);
                shootTimer = 0;
            }

            shootTimer += Time.deltaTime;
        }
    }
}
```

그 다음 아래 사진과 같이 Inspector view에서 Player (Script)에 Laser Prefab에 Laser 프리팝을 연결해주면 끝난다.

![2](/images/04181-2.PNG)
