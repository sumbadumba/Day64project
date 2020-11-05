# Day64project

https://www.clien.net/service/board/lecture/13460871

이곳에서 notepad++ 을 이용해서 대량으로 xml파일을 노가다없이 만드는 가이드를 제시하고 
있음.

void GameImage::SetUVAnimation(int xCount, int yCount, D2D1_SIZE_F offset, float updateInterval, bool loop)
{
	this->xCount = xCount;
	this->yCount = yCount;
	xIndex = 0;
	yIndex = 0;
	uvOffset = offset;

	UpdateInterval = updateInterval;
	LastUpdateTime = -updateInterval;
	//
	useUVAnim = true;
}

void GameImage::UpdateUVAnimation()
{
	float currentTime = Time::Get()->Running();
	float deltaTime = currentTime - LastUpdateTime;
	if (deltaTime < UpdateInterval)
		return;

	srcRect.left = xIndex * uvOffset.width;
	srcRect.top = yIndex * uvOffset.height;
	srcRect.right = srcRect.left + uvOffset.width;
	srcRect.bottom = srcRect.top + uvOffset.height;
	//
	xIndex++;
	if (xIndex >= xCount)
	{
		xIndex = 0;
		yIndex++;

		if (yIndex >= yCount)
		{
			yIndex = 0;
			if (!loopAnim)
				useUVAnim = false;
		}
	}
	//
	LastUpdateTime = currentTime;
}



#pragma once

#include "Image.h"

class GameImage : public Image
{
public:
	GameImage();
	~GameImage();

	void Update() override;
	void Render(D2D1_RECT_F drawRect, Matrix3x2F rotationMatrix = Matrix3x2F::Identity()) override;
	//
	void AddEffect(class ImageEffect* effect) { imageEffect = effect; }

	/*
		float updateInterval : 애니메이션 업데이트 간격(초단위)
	*/
	void SetUVAnimation(int xCount, int yCount, D2D1_SIZE_F offset, float updateInterval, bool loop = false);
	bool UVAnimPlaying() { return useUVAnim; }
	void LoopAnim(bool value) { loopAnim = value; }

private:
	void UpdateUVAnimation();

protected:
	class ImageEffect* imageEffect;

private:
	
	// for uv Animation
	bool useUVAnim;
	bool loopAnim;
	int xCount;
	int yCount;
	int xIndex;
	int yIndex;
	D2D1_SIZE_F uvOffset;
	float UpdateInterval;
	float LastUpdateTime;

};


