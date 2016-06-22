---
layout: post
title: unity5 게임매니저, 텍스트
---

Make-Both-Game-Manager-and-Text

이번엔 게임매니저와 score를 표기해 줄 텍스트를 만들 것이다.

게임매니저란 게임의 전체 상태를 관리하는 것이다.

내가 만들 게임매니저는 enemy의 스폰을 관리 그리고 총알 발사를 관리할 것이다.

빈 오브젝트로 GameManager를 만들어 주고 GameObject - UI - Text로 Text 오브젝트를 만들어 준다.

Text 오브젝트를 더블 클릭해서 Inspector view로 텍스트 사이즈나 폰트등을 원하는 대로 설정해주고

아래 소스와 같이 각각 cs 파일들을 수정해주면 끝난다.

우선 GameManager라는 이름의 cs파일을 만들고 아래 소스를 적어준다.

```c#
using UnityEngine;
using System.Collections;
using UnityEngine.UI;

public class GameManager : MonoBehaviour {

    public static GameManager instance;
    GameObject player;
    int score = 0;
    public Text scoreText;

    void Awake()
    {
        if (GameManager.instance == null)
        {
            GameManager.instance = this;
        }
    }

	// Use this for initialization
	void Start () {
        player = GameObject.FindWithTag("Player");
        Invoke("StartGame", 3f);
	}
	
	// Update is called once per frame
	void Update () {
	
	}

    void StartGame()
    {
        player.GetComponent<Player>().canShoot = true;
        SpawnManager.instance.isSpawn = true;
    }

    public void AddScore(int enemyScore) {
        score += enemyScore;
        scoreText.text = "score : " + score;
    }
}
```

다음으로 Player.cs 파일을 수정해준다.

```c#
using UnityEngine;
using System.Collections;

public class Player : MonoBehaviour
{

    public float moveSpeed = 0.5f;
    public GameObject explosionPrefab;
    public GameObject laserPrefab;
    public bool canShoot = false;
    public float shootDelay = 0.2f;
    public float shootTimer = 0.2f;

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

그리고 Enemy.cs파일

```c#
using UnityEngine;
using System.Collections;

public class Enemy : MonoBehaviour {
    public int killScore = 100;
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
            SoundManager.instance.PlaySound();
            Destroy(col.gameObject);
            Destroy(this.gameObject);
            GameManager.instance.AddScore(killScore);
        }
    }

}
```

마지막으로 SpawnManager.cs 파일을 수정해준다.

```c#
using UnityEngine;
using System.Collections;

public class SpawnManager : MonoBehaviour {

    Vector3[] positions = new Vector3[5];
    public GameObject enemyPrefab;
    public bool isSpawn = true;
    public float spawnDelay = 1.5f;
    float spawnTimer = 0f;
    public static SpawnManager instance;

    void Awake()
    {
        if (SpawnManager.instance == null)
        {
            SpawnManager.instance = this;
        }
    }

	// Use this for initialization
	void Start () {
        CreatePosition();
	}
	
	// Update is called once per frame
	void Update () {
        SpawnEnemy();
	}

    void CreatePosition() {
        float viewPosY = 1.2f;
        float viewPosX = 0f;
        float gapX = 1f / 6f;

        for(int i = 0; i < positions.Length; i++) {
            viewPosX = gapX + gapX * i;
            Vector3 viewPos = new Vector3(viewPosX, viewPosY, 0);
            Vector3 worldPos = Camera.main.ViewportToWorldPoint(viewPos);
            worldPos.z = 0f;
            positions[i] = worldPos;
        }
    }

    void SpawnEnemy()
    {
        if (isSpawn)
        {
            if (spawnTimer > spawnDelay)
            {
                int rand = Random.Range(0, positions.Length);
                Instantiate(enemyPrefab, positions[rand], Quaternion.identity);
                spawnTimer = 0f;
            }
            spawnTimer += Time.deltaTime;
        }
    }
}
```

그리고 아까 만든 Text 오브젝트를 GameManager 오브젝트의 GameManager 컴포넌트에 연결해주면 끝난다.
