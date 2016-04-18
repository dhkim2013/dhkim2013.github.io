---
layout: post
title: unity5 레이저 설정
---

Unity5-Setting-Laser

이번에는 저번에 만들었던 레이저에 약간의 기능을 추가할 것이다.

*스페이스바 누르면 레이저 발사

*레이저와 Enemy가 충돌하면 폭발

*레이저가 화면 밖으로 나가면 삭제

스페이스바를 누르면 레이저가 발사 되도록 하려면 Player 오브젝트에 연결되어 있는 스크립트를 아래와 같이 수정하면 된다.

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

            if (shootTimer > shootDelay && Input.GetKey(KeyCode.Space))
            {
                Instantiate(laserPrefab, transform.position, Quaternion.identity);
                shootTimer = 0;
            }

            shootTimer += Time.deltaTime;
        }
    }

}
```

그 다음에 레이저와 Enemy 오브젝트가 만나면 폭발하게 하려면 Enemy 오브젝트에 연결되어 있는 스크립트를 아래와 같이 수정하면 된다.

```c#
using UnityEngine;
using System.Collections;

public class Enemy : MonoBehaviour {

    public float moveSpeed = 0.5f;
    public GameObject explosionPrefab;

    void moveControl()
    {
        float yMove = moveSpeed * Time.deltaTime;
        transform.Translate(0, -yMove, 0);
    }

	// Use this for initialization
	void Start () {

	}
	
	// Update is called once per frame
	void Update () {
        moveControl();
	}

    void OnTriggerEnter2D(Collider2D col)
    {
        if (col.gameObject.tag == "Laser")
        {
            Instantiate(explosionPrefab, transform.position, Quaternion.identity);
            Destroy(col.gameObject);
            Destroy(this.gameObject);
        }
    }

}
```

스크립트를 수정한 후에 unity로 돌아가서 아래 사진과 같이 Enemy 오브젝트의 Inspector view에 Enemy 스크립트에 있는 Explosion Prefab에 Explosion 프리팝을 연결해야 한다.

![1](/images/04182-1.PNG)

그 다음 레이저가 화면 밖으로 나가면 삭제하게 하려면 Laser 스크립트의 내용을 아래와 같이 수정하면 된다.

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

    void OnBecameInvisible()
    {
        Destroy(this.gameObject);
    }

}
```

OnBecamInvisible()이라는 함수는 해당 스크립트가 연결되어 있는 오브젝트가 화면 밖으로 나가면 자동으로 호출 되는 함수다.
