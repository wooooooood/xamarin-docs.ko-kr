---
title: Configuration Manager에서 비활성화된 확인란 배포
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
author: davidortinau
ms.author: daortin
ms.date: 12/02/2016
ms.openlocfilehash: edf471f1d9a2ee4adc11f09e0c7b7ad3cf6f78f1
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014250"
---
# <a name="deploy-checkboxes-disabled-in-configuration-manager"></a>Configuration Manager에서 비활성화된 확인란 배포

Xamarin 3.5부터 Xamarin.ios 프로젝트는 **시작** 도구 모음 단추를 누르거나 **디버그 > 디버깅 시작** 메뉴 항목을 선택할 때마다 자동으로 배포 됩니다. 이러한 명령 중 하나를 실행 하기 전에 원하는 Xamarin.ios 앱 프로젝트를 **시작 프로젝트로** 설정 해야 합니다.

이로 인해 Xamarin.ios 프로젝트에 대 한 Visual Studio Configuration Manager에서 **배포** 확인란은 의도적으로 비활성화 되어 있습니다.

![](deploy-checkboxes-images/configuration.png "Visual Studio Configuration Manager showing the 'Deploy' checkbox disabled for a Xamarin.iOS project in Xamarin 3.5")

이 변경으로 인해 Xamarin.ios 앱 프로젝트를 배포 하도록 설정 하지 않은 경우 이전 버전의 Xamarin (버전 3.3 및 이전 버전)에 나타날 수 있는 오류가 제거 됩니다.

![](deploy-checkboxes-images/error.png "Error dialog: The project iPhoneApp1 needs to be deployed before it can be started. Verify the project is selected to be deployed in the Solution Configuration Manager.")
