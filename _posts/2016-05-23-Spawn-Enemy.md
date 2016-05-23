---
layout: post
title: unity5 spawn enemy
---

Enemy-Spawn

이번에는 매 프레임마다 자동으로 enemy를 생성되게 만들거다.

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
