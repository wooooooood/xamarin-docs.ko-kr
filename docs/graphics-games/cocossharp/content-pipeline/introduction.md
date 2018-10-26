---
title: 콘텐츠 파이프라인 소개
description: 콘텐츠 파이프라인은 응용 프로그램 또는 응용 프로그램의 일부를 사용 되는 게임 프로젝트에서 로드할 수 있는 형식으로 파일을 변환 합니다. MonoGame Content Pipeline에 CocosSharp 및 MonoGame 프로젝트에 대 한 파일을 변환 하기 위한 특정 콘텐츠 파이프라인 구현입니다.
ms.prod: xamarin
ms.assetid: 40628B5F-FAF7-4FA7-A929-6C3FEA83F8EC
author: conceptdev
ms.author: crdun
ms.date: 03/27/2017
ms.openlocfilehash: 712c430fb6309ba0f5c3e573267c59e422de8ad2
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104143"
---
# <a name="introduction-to-content-pipelines"></a>콘텐츠 파이프라인 소개

_콘텐츠 파이프라인은 응용 프로그램 또는 응용 프로그램의 일부를 사용 되는 게임 프로젝트에서 로드할 수 있는 형식으로 파일을 변환 합니다. MonoGame Content Pipeline에 CocosSharp 및 MonoGame 프로젝트에 대 한 파일을 변환 하기 위한 특정 콘텐츠 파이프라인 구현입니다._

이 문서에서는 주로 집중 콘텐츠 파이프라인에 대 한 개념적 이해를 제공 합니다 *MonoGame Content Pipeline*, CocosSharp 및 MonoGame을 사용 하는 콘텐츠 파이프라인 구현에는 합니다.


## <a name="what-is-a-content-pipeline"></a>콘텐츠 파이프라인 이란?

용어 *콘텐츠 파이프라인* 파일 형식을 변환 하는 프로세스에 대 한 일반 용어입니다. 합니다 *입력* 콘텐츠 파이프라인은 일반적으로 Photoshop에서 이미지 파일 같은 도구를 제작 하 여 출력 파일입니다. 콘텐츠 파이프라인을 만듭니다는 *출력* 게임 프로젝트에서 직접 로드 될 수 있는 형식의 파일입니다. 일반적으로 출력 파일 빠른 로드에 최적화 된 및 디스크 크기를 축소 합니다.

콘텐츠 파이프라인은 명령줄 수 실행 파일, 전용 GUI 기반 응용 프로그램 또는 플러그 인에에서 포함 된 Visual Studio와 같은 다른 응용 프로그램입니다. 콘텐츠 파이프라인에는 일반적으로 게임 실행 되기 전에 실행 합니다. 콘텐츠 파이프라인은 Visual Studio와 같은 일부 응용 프로그램과 연결, 컴파일 시간 동안 실행할 수 있습니다. 독립 실행형 응용 프로그램 콘텐츠 파이프라인을 사용 하는 경우에 이렇게 하려면 사용자가 명시적으로 지정 하는 경우 실행할 수 있습니다. 응용 프로그램 또는 특정 입력된 파일을 변환 하는 일을 담당 하는 논리 (같은 **.png**)는 연결 된 출력을 파일 이라고는 *작성기*합니다. 

다음과 같이 런타임 시 로드를 제작 하는 파일의 경로 시각화할 수 했습니다.

![](introduction-images/image1.png "런타임 시 로드를 제작 하는 파일 경로이 다이어그램에서 시각화")

## <a name="why-use-a-content-pipeline"></a>콘텐츠 파이프라인을 사용 하는 이유

콘텐츠 파이프라인 간에 컴파일 시간을 높이고 개발 프로세스에 복잡성을 추가할 수 있는 제작 응용 프로그램 및 게임을 별도 단계를 소개 합니다. 이러한 고려 사항에도 불구 하 고 콘텐츠 파이프라인 소개 다양 한 이점 게임 개발:


### <a name="converting-to-a-format-understood-by-the-game"></a>게임에서 인식 한 형식으로 변환

CocosSharp 및 MonoGame 다양 한 유형의 콘텐츠를 로드 하기 위한 메서드 제공 그러나 콘텐츠를 로드 하기 전에 올바르게 포맷 해야 합니다. 대부분의 콘텐츠 형식을 특정 유형의 로드 하기 전에 변환 해야 합니다. 예를 들어 음향 효과에 **.wav** 형식으로 변환 해야는 **.xnb** CocosSharp 및 MonoGame 로드 지원 하지 않으므로 런타임에 로드 되도록 파일을 **.wav** 파일 형식입니다.


### <a name="converting-to-a-format-native-to-the-hardware"></a>하드웨어를 네이티브 형식으로 변환

다른 하드웨어 런타임에 콘텐츠를 다르게 처리할 수 있습니다. 예를 들어 통해 CocosSharp 게임을 만들 때 이미지 파일을 로드할 수는 `CCSprite` 인스턴스. IOS 및 Android 모두에서 파일을 로드 하려면 동일한 코드를 사용할 수 있지만 각 플랫폼 다르게 로드 된 파일을 저장 합니다. 결과적으로 MonoGame 콘텐츠 파이프라인은 형식 질감 **.xnb** 대상 플랫폼에 따라 다르게 파일입니다.


### <a name="reducing-size-on-disk"></a>디스크의 크기를 줄임으로써 

콘텐츠 파이프라인 정보를 제거할 수는 작성 시 유용 하지만 런타임 시 필요 하지 않습니다. 원본 (입력된) 파일에서 기존 콘텐츠를 유지 관리 하는 콘텐츠 작성자가 도움이 되는 모든 정보를 저장할 수 있지만 출력 파일의 전체 파일을 작게 유지 하는 축소 된 기본 수 있습니다. 모바일 게임 설치 미디어에 배포 하지 않고 다운로드에 대 한이 고려 사항은 특히 유용 합니다.


### <a name="reducing-load-time"></a>로드 시간 단축

게임에는 런타임 성능 향상을 위해 시각적 개체를 향상 시키거나 새 기능을 추가 하는 콘텐츠를 수정 해야 할 수 있습니다. 예를 들어 많은 3D 게임에 한 번 조명 계산 다음 복잡 한 장면을 렌더링 하는 경우이 계산의 결과 사용 합니다. 많은 비용이 들거나 실행 될 수 있는 콘텐츠를 로드할 경우 이러한 계산을 수행 하는 이후 계산 대신 수행할 수 있습니다 게임을 빌드할 때. 결과 계산의 콘텐츠를 할 수 있는 것 보다 훨씬 빠르게 로드 수를 사용 하도록 설정 하면 콘텐츠를 포함할 수 있습니다. 


## <a name="xnb-file-extension"></a>xnb 파일 확장명

합니다 **.xnb** 파일 확장명은 Monogame 콘텐츠 파이프라인에 의해 출력 된 모든 파일의 확장명입니다. 이 Microsoft XNA 콘텐츠 파이프라인에서 출력 파일의 확장명과 일치 합니다.

합니다 **.xnb** 원래 파일 형식에 관계 없이 확장을 사용 합니다. 즉, 이미지 파일 (**.png**), 오디오 파일 (**.wav**), 및 모든 사용자 지정 파일 형식을 모든 출력으로 **.xnb** 콘텐츠 파이프라인을 통해 전달 될 때 파일입니다. 다른 파일 형식 모두 CocosSharp MonoGame 메서드와 로드 하는 다음 구분 하기 위해 확장을 사용할 수 없으므로 **.xnb** 파일을 로드할 때 파일 확장명 기다리지 않습니다.

CocosSharp 및 MonoGame.xnb 파일 설명 하는 Monogame 파이프라인 도구를 사용 하 여 만들 수 있습니다 [이 연습의](~/graphics-games/cocossharp/content-pipeline/walkthrough.md)합니다.


## <a name="summary"></a>요약

이 문서에서는 제공 개요 및 콘텐츠 파이프라인의 이점을 일반적으로 MonoGame 콘텐츠 파이프라인 소개와 합니다.

## <a name="related-links"></a>관련 링크

- [MonoGame 파이프라인 설명서](http://www.monogame.net/documentation/?page=Pipeline)
