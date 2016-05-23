---
layout: post
title: unity5 spawn enemy
---

Enemy-Spawn

이번에는 매 프레임마다 자동으로 enemy를 생성되게 만들거다.

만들기 이전에 뷰포트와 월드포인트의 개념을 알아야한다.

뷰포트는 화면을 (0, 0)부터 (1, 1)까지로 보게하여 사람이 알기 편하게 해준다.

그렇게 때문에 우리는 뷰포트를 사용할 것이다.

하지만 유니티는 월드포인트로 처리하기 때문에 뷰포트를 월드포인트로 바꿔야 한다.

그러기 위해서 아래 소스에서는 Camera.main.ViewportToWorldPoint() 메소드를 사용한다.

Camera.main.ViewportToWorldPoint 메소드는 Vector3의 뷰포트를 포지션을 월드포인트로 바꿔주는 메소드다.

개념을 이해했다면 이제 만들도록 하자.

전에 만든 Enemy 오브젝트를 프리팹으로 만들고 Create Empty로 SpawnManager라는 이름의 빈 오브젝트를 만든다.

그리고 SpawnManager라는 스크립트도 만들어주고 아래 소스를 입력하고 SpawnManger 오브젝트에 SpawnManager 스크립트를 연결해주고 프리팹을 연결해주면 끝난다.

```c#
using UnityEngine;
using System.Collections;

public class SpawnManager : MonoBehaviour {

    Vector3[] positions = new Vector3[5];
    public GameObject enemyPrefab;
    public bool isSpawn = true;
    public float spawnDelay = 1.5f;
    float spawnTimer = 0f;

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
