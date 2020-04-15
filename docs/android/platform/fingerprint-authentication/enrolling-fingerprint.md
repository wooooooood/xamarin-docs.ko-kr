---
title: 지문 등록
description: Android 디바이스 또는 에뮬레이터에서 화면 잠금을 설정하고 지문을 등록하는 방법입니다.
ms.prod: xamarin
ms.assetid: 52092F63-00EE-4F8B-A49F-65C9CCBA7EF2
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: c0290dfa3b4aa301a07a589f78577899e8282158
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027590"
---
# <a name="enrolling-a-fingerprint"></a>지문 등록

## <a name="enrolling-a-fingerprint-overview"></a>지문 등록 개요

디바이스가 이미 지문 인증을 사용하여 구성된 경우에만 Android 애플리케이션에서 지문 인증을 활용할 수 있습니다. 이 가이드에서는 Android 디바이스 또는 에뮬레이터에 지문을 등록하는 방법을 설명합니다. 에뮬레이터는 실제 하드웨어를 사용하여 지문 스캔을 수행하지 않지만 Android Debug Bridge(아래 설명 참조)의 도움으로 지문 스캔을 시뮬레이션할 수 있습니다.  이 가이드에서는 Android 디바이스에서 화면 잠금을 사용하도록 설정하고 인증을 위해 지문을 등록하는 방법을 설명합니다.

## <a name="requirements"></a>요구 사항

지문을 등록하려면 Android 디바이스 또는 API 레벨 23(Android 6.0)을 실행하는 에뮬레이터가 있어야 합니다.

ADB(Android Debug Bridge)를 사용하려면 명령 프롬프트를 잘 알고 있어야 하며, **adb** 실행 파일은 Bash, PowerShell 또는 명령 프롬프트 환경의 경로에 있어야 합니다.

## <a name="configuring-a-screen-lock-and-enrolling-a-fingerprint"></a>화면 잠금 구성 및 지문 등록 

화면 잠금을 설정하려면 다음 단계를 수행합니다.

1. **설정 > 보안**으로 이동하여 **화면 잠금**을 선택합니다.

    ![보안 화면에서 화면 잠금 선택 항목의 위치](enrolling-fingerprint-images/testing-01.png)

2. 표시되는 다음 화면에서 화면 잠금 보안 방법 중 하나를 선택하고 구성할 수 있습니다. 

    ![살짝 밀기, 패턴, PIN 또는 암호 선택](enrolling-fingerprint-images/testing-02.png)

   사용 가능한 화면 잠금 방법 중 하나를 선택하여 완료합니다.

3. 화면 잠금을 구성한 후 **설정 > 보안** 페이지로 돌아가서 **지문**을 선택합니다.

    ![보안 화면에서 지문 선택 항목의 위치](enrolling-fingerprint-images/testing-03.png)

4. 여기에서 시퀀스를 따라 디바이스에 지문을 추가합니다.

    [![디바이스에 지문을 추가하기 위한 스크린샷 시퀀스](enrolling-fingerprint-images/testing-04-sml.png)](enrolling-fingerprint-images/testing-04.png#lightbox)

5. 최종 화면에서 지문 스캐너에 손가락을 대라는 메시지가 표시됩니다. 

    ![센서에 손가락을 대라는 메시지가 표시된 화면](enrolling-fingerprint-images/testing-05.png)

    Android 디바이스를 사용하는 경우 스캐너에 손가락을 터치하여 프로세스를 완료합니다. 

### <a name="simulating-a-fingerprint-scan-on-the-emulator"></a>에뮬레이터에서 지문 스캔 시뮬레이션

Android 에뮬레이터에서 Android Debug Bridge를 사용하여 지문 스캔을 시뮬레이션할 수 있습니다. OS X에서는 터미널 세션을 시작하고 Windows에서는 명령 프롬프트 또는 Powershell 세션을 시작하여 `adb`를 실행합니다.

```shell
$ adb -e emu finger touch 1
```

값 **1**은 "스캔된" 손가락의 _finger\_id_입니다. 각 가상 지문으로 할당하는 고유한 정수입니다. 나중에 앱이 실행되는 경우 에뮬레이터에서 지문을 확인할 때마다 동일한 ADB 명령을 실행할 수 있습니다. `adb` 명령을 실행하고 _finger\_id_를 전달하여 지문 스캔을 시뮬레이션할 수 있습니다.

지문 스캔이 완료된 후 Android는 사용자에게 지문이 추가되었음을 알립니다.  

![지문 추가 완료 화면](enrolling-fingerprint-images/testing-06.png)

## <a name="summary"></a>요약 

이 가이드에서는 Android 디바이스 또는 Android 에뮬레이터에서 화면 잠금을 설정하고 지문을 등록하는 방법을 설명했습니다. 
