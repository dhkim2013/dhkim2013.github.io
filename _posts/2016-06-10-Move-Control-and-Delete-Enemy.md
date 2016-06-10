---
layout: post
title: unity5 Player 움직임 처리와 Enemy 삭제
---

Player-Move-Control-and-Delete-Enemy

이번엔 Player의 상하 움직임 추가와 화면에 가두기 그리고 Enemy가 화면에서 벗어나면 삭제되게 만들 것이다.

우선 Player를 화면에 가두고 상하 움직임을 추가하기 위해선 아래 소스로 수정하면 된다.

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
        float moveY = moveSpeed * Time.deltaTime * Input.GetAxis("Vertical");

        transform.Translate(moveX, moveY, 0);

        Vector3 viewPos = Camera.main.WorldToViewportPoint(transform.position);
        viewPos.x = Mathf.Clamp01(viewPos.x);
        viewPos.y = Mathf.Clamp01(viewPos.y);

        Vector3 worldPos = Camera.main.ViewportToWorldPoint(viewPos);
        transform.position = worldPos;
    }

    void OnTriggerEnter2D(Collider2D col)
    {
        if (col.gameObject.tag == "Enemy")
        {
            Instantiate(explosionPrefab, transform.position, Quaternion.identity);
            SoundManager.instance.PlaySound();
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

Enemy 삭제는 OnBecamInvisible을 사용하지 않고 다른 방법으로 삭제할 것이다.

빈 게임오브젝트를 추가하고 Rigidbody 2D와 Box Collider 2D 컴포넌트를 추가해주고,

Is Trigger와 Is Kinematic을 체크해준다.

그리고 Enemy가 삭제되었으면 하는 위치에 배치하고 사이즈도 임의로 조정한 뒤에 아래 소스를 연결해주면 된다.

```c#
using UnityEngine;
using System.Collections;

public class RemoveZone : MonoBehaviour {

	// Use this for initialization
	void Start () {
	
	}
	
	// Update is called once per frame
	void Update () {

	}

    void OnTriggerEnter2D(Collider2D collider2D)
    {
        if (collider2D.gameObject.tag == "Enemy")
        {
            Destroy(collider2D.gameObject);
        }
    }
}
```
