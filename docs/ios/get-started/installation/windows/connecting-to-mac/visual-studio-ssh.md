---
title: 내 Mac 위치
description: Xamarin.iOS 프로젝트를 빌드하도록 Visual Studio에 대해 Mac을 구성하는 방법에 대한 지침
ms.prod: xamarin
ms.assetid: 4f792690-b3b1-4d56-a5e2-363b4f246059
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: f3e2988c9700549db24ad69277df9e412c99caed
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="wheres-my-mac"></a>내 Mac 위치

_Xamarin.iOS 프로젝트를 빌드하도록 Visual Studio에 대해 Mac을 구성하는 방법에 대한 지침_

현재 **안정적인** Visual Studio용 Xamarin 버전은 이전 버전과 다른 방식으로 Mac과 상호 작용합니다[^](#earlier-versions).

Mac을 Xamarin Mac Agent로 사용하려면 Mac에서 **원격 로그인**을 사용하도록 설정해야 합니다.

1. *스포트라이트*를 열고(`Cmd-Space`) **원격 로그인**을 검색한 후 **공유** 결과를 선택합니다. 그러면 **공유** 패널에 **시스템 환경 설정**이 열립니다.

  ![](visual-studio-ssh-images/spotlight.png "원격 로그인에 대한 스포트라이트 검색")

2. 왼쪽에 있는 **서비스** 목록에서 **원격 로그인**을 선택하여 Visual Studio용 Xamarin이 Mac에 연결할 수 있도록 허용합니다.

  ![](visual-studio-ssh-images/sharing.png "서비스 목록에서 원격 로그인 옵션 선택")

3. **모든 사용자**에 대한 액세스를 허용하도록 또는 Mac 사용자/그룹이 오른쪽에 있는 허용되는 사용자 목록에 포함되도록 **원격 로그인**을 설정합니다.

이제 Mac이 동일한 네트워크에 있으면 Visual Studio에서 Mac을 검색할 수 있습니다.
Visual Studio가 Mac에 연결하려고 시도하면 Visual Studio에서 Mac 사용자 자격 증명을 사용하여 로그인하라는 메시지가 표시됩니다.

## <a name="where-can-i-find-more-information"></a>추가 정보는 어디서 볼 수 있나요?

새 에이전트를 설치하고 이해하는 데 도움이 될 만한 여러 지침이 아래에 나열되어 있습니다.

- [Mac에 연결](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
- [문제 해결](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)
- [Xamarin University - Xamarin Mac Agent](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)

<a name="earlier-versions" />

## <a name="migrating-from-previous-versions"></a>이전 버전에서 마이그레이션

이전 버전의 Visual Studio용 Xamarin을 사용하신 분들은 Mac에서 시작한 후 PIN 번호를 통해 Visual Studio 인스턴스와 *연결*해야 하는 **Xamarin.iOS 빌드 호스트** 응용 프로그램에 익숙하실 것입니다.

이 버전의 Visual Studio용 Xamarin에서는 더 이상 빌드 호스트 응용 프로그램이 필요 없습니다.
