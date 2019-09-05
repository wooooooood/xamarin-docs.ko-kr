---
title: Configuration Manager에서 비활성화된 확인란 배포
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
author: conceptdev
ms.author: crdun
ms.date: 12/02/2016
ms.openlocfilehash: 82ff1a684ffad75a301f0db6b0f8e3116be6746d
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70285062"
---
# <a name="deploy-checkboxes-disabled-in-configuration-manager"></a>Configuration Manager에서 비활성화된 확인란 배포

Xamarin 3.5부터 Xamarin.ios 프로젝트는 **시작** 도구 모음 단추를 누르거나 **디버그 > 디버깅 시작** 메뉴 항목을 선택할 때마다 자동으로 배포 됩니다. 이러한 명령 중 하나를 실행 하기 전에 원하는 Xamarin.ios 앱 프로젝트를 **시작 프로젝트로** 설정 해야 합니다.

이로 인해 Xamarin.ios 프로젝트에 대 한 Visual Studio Configuration Manager에서 **배포** 확인란은 의도적으로 비활성화 되어 있습니다.

![](deploy-checkboxes-images/configuration.png "Xamarin 3.5에서 Xamarin.ios 프로젝트에 대해 사용 하지 않도록 설정 된 ' 배포 ' 확인란을 보여 주는 Visual Studio Configuration Manager")

이 변경으로 인해 Xamarin.ios 앱 프로젝트를 배포 하도록 설정 하지 않은 경우 이전 버전의 Xamarin (버전 3.3 및 이전 버전)에 나타날 수 있는 오류가 제거 됩니다.

![](deploy-checkboxes-images/error.png "오류 대화 상자: IPhoneApp1 프로젝트를 시작 하려면 먼저 배포 해야 합니다. 솔루션 Configuration Manager에 배포 하도록 프로젝트를 선택 했는지 확인 합니다.")
