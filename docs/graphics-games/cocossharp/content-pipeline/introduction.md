---
title: 콘텐츠 파이프라인 소개
description: 콘텐츠 응용 프로그램 또는 응용 프로그램의 일부 파이프라인은 데 사용 되는 파일 게임 프로젝트에서 로드할 수 있는 형식으로 변환 합니다. MonoGame 콘텐츠 파이프라인에 CocosSharp 및 MonoGame 프로젝트에 대 한 파일을 변환 하기 위한 특정 콘텐츠 파이프라인 구현입니다.
ms.prod: xamarin
ms.assetid: 40628B5F-FAF7-4FA7-A929-6C3FEA83F8EC
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: a369c5ba61033eb61c0f188c03b21e08c71784fb
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33922837"
---
# <a name="introduction-to-content-pipelines"></a>콘텐츠 파이프라인 소개

_콘텐츠 응용 프로그램 또는 응용 프로그램의 일부 파이프라인은 데 사용 되는 파일 게임 프로젝트에서 로드할 수 있는 형식으로 변환 합니다. MonoGame 콘텐츠 파이프라인에 CocosSharp 및 MonoGame 프로젝트에 대 한 파일을 변환 하기 위한 특정 콘텐츠 파이프라인 구현입니다._

이 문서에서는 콘텐츠 파이프라인에 주로 초점의 개념적 이해는 *MonoGame 콘텐츠 파이프라인*, CocosSharp 및 MonoGame와 함께 사용 하는 콘텐츠 파이프라인 구현인 합니다.


## <a name="what-is-a-content-pipeline"></a>콘텐츠 파이프라인 이란?

용어 *콘텐츠 파이프라인* 파일 형식을 다른 형식으로 변환 하는 과정에 대 한 일반적인 용어입니다. *입력* 콘텐츠 파이프라인의는 일반적으로 Photoshop에서 이미지 파일 등의 제작 도구에 의해 출력 된 파일입니다. 파이프라인은 콘텐츠 만듭니다는 *출력* 게임 프로젝트에서 직접 로드할 수 있는 형식의 파일입니다. 일반적으로 출력 파일은 빠른 로드를 위해 최적화 되었으며 디스크 크기를 줄이기.

파이프라인은 명령줄 수 콘텐츠 실행 파일, 전용 GUI 기반 응용 프로그램 또는 플러그 인에에서 포함 된 Visual Studio와 같은 다른 응용 프로그램입니다. 콘텐츠 파이프라인에는 일반적으로 게임을 실행 하기 전에 실행 합니다. 콘텐츠 파이프라인은 Visual Studio와 같은 일부 응용 프로그램과 연결, 컴파일 시간 동안 실행 될 수 있습니다. 콘텐츠 파이프라인은 독립 실행형 응용 프로그램, 사용자가 작업을 수행 하도록 명시적으로 지시 받고 때 실행 될 수 있습니다. 응용 프로그램 또는 특정 입력된 파일을 변환 해야 하는 논리 (같은 **.png**) 연결 된 출력 파일 라고 하는 *작성기*합니다. 

파일을 다음과 같이 런타임 시 로드 되 고 제작에서 작업을 수행 하는 경로 시각화할 수 있습니다.

![](introduction-images/image1.png "이 다이어그램에서 파일을 제작 런타임에 로드할 대상에서 작업을 수행 하는 경로 표시")

## <a name="why-use-a-content-pipeline"></a>콘텐츠 파이프라인을 사용 하는 이유

콘텐츠 파이프라인 간에 컴파일 시간 증가 하 고 개발 프로세스에 복잡성이 증가할 수 있는 제작 응용 프로그램 및 게임을 별도 단계를 소개 합니다. 이러한 고려 사항에도 불구 하 고 콘텐츠 파이프라인 게임 개발 다양 한 이점이 소개:


### <a name="converting-to-a-format-understood-by-the-game"></a>게임에서 인식 한 형식으로 변환

CocosSharp 및 MonoGame 제공 다양 한 유형의 콘텐츠를 로드 하기 위한 메서드 그러나 콘텐츠 로드 되기 전에 정확히 지정 해야 합니다. 대부분의 콘텐츠 형식을 몇 가지 유형의 로드 하기 전에 변환 해야 합니다. 예를 들어에서 소리 효과 **.wav** 형식으로 변환 해야는 **.xnb** CocosSharp 및 MonoGame 로드 지원 하지 않으므로 런타임에 로드할된 파일의 **.wav** 파일 형식입니다.


### <a name="converting-to-a-format-native-to-the-hardware"></a>하드웨어의 기본 형식으로 변환

다른 하드웨어 런타임에 콘텐츠를 다르게 취급할 수 있습니다. 예를 들어 통해 CocosSharp 게임을 만들 때 파일 이미지를 로드할 수는 `CCSprite` 인스턴스. IOS 및 Android를 둘 다에서 파일을 로드 하는 동일한 코드를 사용할 수 있지만 각 플랫폼 로드 된 파일이 다르게 저장 합니다. 이 인해 MonoGame 콘텐츠 파이프라인 서식을 질감 **.xnb** 대상 플랫폼에 따라 다르게 파일입니다.


### <a name="reducing-size-on-disk"></a>디스크 크기 줄이기 

콘텐츠 정보를 제거 하려면 파이프라인을 사용할 수 있는 작성자 시 유용 하지만 런타임 시 필요 하지 않습니다. 원본 (입력된) 파일 콘텐츠 작성자가 기존 콘텐츠를 유지 관리할 수 있는 모든 정보를 저장할 수 있지만 출력 파일 전체 게임 파일을 작게 유지 하는 축소 될 수 있습니다. 설치 미디어에 배포 하지 않고 다운로드 하는 모바일 게임에 대 한이 고려 사항은 특히 유용 합니다.


### <a name="reducing-load-time"></a>로드 시간 단축

게임을 시각적 개체, 향상 시키거나 새 기능을 추가할 런타임 성능을 향상 시키기 위해 콘텐츠를 수정 해야 할 수 있습니다. 예를 들어 많은 3D 게임 한 번 조명 계산 다음 복잡 한 장면을 렌더링 하는 경우이 계산의 결과 사용 합니다. 비용이 들 수 있습니다 콘텐츠를 로드할 때 이러한 계산을 수행 하는 이후 계산 대신 수행할 수 있습니다는 게임을 빌드할 때. 결과 계산에 가능한 것 보다 훨씬 더 빠르게 로드 되도록 콘텐츠를 사용 하도록 설정 된 콘텐츠를 포함할 수 있습니다. 


## <a name="xnb-file-extension"></a>xnb 파일 확장명

**.xnb** 파일 확장명은 Monogame 콘텐츠 파이프라인에 의해 출력 된 모든 파일에 대 한 확장입니다. Microsoft XNA 콘텐츠 파이프라인에 의해 출력 된 파일의 확장명을 일치 합니다.

**.xnb** 확장명이 원본 파일 형식에 관계 없이 사용 됩니다. 즉, 이미지 파일 (**.png**), 오디오 파일 (**.wav**), 사용자 정의 파일 형식의 모든 출력할으로 및 **.xnb** 콘텐츠 파이프라인을 통해 전달 될 때 파일입니다. 다른 파일 형식 모두 CocosSharp MonoGame 메서드와 로드 하는 다음 이후 간을 서로 구별 하는 확장을 사용할 수 없습니다 **.xnb** 파일을 로드할 때 파일을 extensions 기다리지 않습니다.

설명 Monogame 파이프라인 도구를 사용 하 여 CocosSharp 및 MonoGame.xnb 파일을 만들 수 있습니다 [이 연습에서](~/graphics-games/cocossharp/content-pipeline/walkthrough.md)합니다.


## <a name="summary"></a>요약

이 문서의 제공 개요 및 콘텐츠 파이프라인의 이점 일반적으로 MonoGame 콘텐츠 파이프라인에 대 한 소개 및.

## <a name="related-links"></a>관련된 링크

- [MonoGame 파이프라인 설명서](http://www.monogame.net/documentation/?page=Pipeline)
