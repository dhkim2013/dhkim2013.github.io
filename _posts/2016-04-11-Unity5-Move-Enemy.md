---
layout: post
title: unity5 enemy 이동
---

Move-Enemy

이번에는 unity5로 Enemy를 생성하고 이동하는 것을 구현할 것이다.

![1](/images/041101.PNG)

_-Sprite image of Enemy's Inspector view_

Enemy를 생성하기 전에 위와 같이 Enemy로 할 스프라이트 이미지의 기본 설정을 해주었다.

위의 사진을 자세히 설명하자면 Texture Type은 2D 스프라이트 이미지이기 때문에 Sprite (2D and UI)로 설정해주었다.


Sprite Mode는 스프라이트 이미지에 2개 이상의 이미지가 들어있기 때문에 Multiple로 설정해주었다.


Generate Mip Maps는 3D로 하는게 아니기 때문에 체크를 해제했다.


Max Size는 내가 사용할 이미지의 크기가 32*16 사이즈라서 32로 지정했다.

이후에 Sprite Editor를 눌러서 자신의 이미지에 따라 알맞게 이미지를 슬라이스 해주면 된다.

![2](/images/041102.PNG)

_-Project view_

설정을 마친 후에 Project view에서

![3](/images/041103.PNG)

자신이 설정한 이미지의 오른쪽에 있는 재생모양을 누르면

![4](/images/041104.PNG)

위와 같이 이미지가 잘 나누어 졌는지 확인이 가능하다.

확인을 마쳤으면, 나누어진 이미지를 하나하나 Ctrl + Click 해주어서 모두 선택한 후에 Hierarchy view에 드래그를 해주면 
저장을 하라고 나온다.

Enemy라고 저장을 하면 

![5](/images/041107.PNG)

![6](/images/041108.PNG)

위와 같이 파일들이 Project view에 생기며 Hierarchy view에도 Enemy라는 오브젝트가 생긴다.

Enemy를 화면에 배치한 후에 play하면 Enemy의 애니메이션이 실행되는 것을 볼 수가 있다.

애니메이션의 변환 속도를 조절하고 싶으면

![7](/images/041108.PNG)

위의 애니메이션 컨트롤러를 더블클릭 해주면 

![8](/images/041105.PNG)

위와 같은 Animator view가 생긴다.

Alt를 누르고 마우스를 클릭한 채로 마우스를 움직이면 Animator view에서 화면을 이동할 수 있다.

위의 사진과 같은 Enemy를 누르면 

![9](/images/041106.PNG)

오른쪽에 위와 같은 Inspector view가 생길 것이다.

저기서 Speed 값을 조절해주면 애니메이션 변환 속도가 바뀐다.

이제 Enemy를 위에서 아래로 움직이는 Script를 생성해서 Enemy 오브젝트와 연결 해주면 끝난다.

```c#
using UnityEngine;
using System.Collections;

public class Enemy : MonoBehaviour {

    public float moveSpeed = 0.5f;

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
}
```


