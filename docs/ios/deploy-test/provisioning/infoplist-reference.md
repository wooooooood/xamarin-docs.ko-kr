---
title: "Info.plist 참조"
ms.topic: article
ms.prod: xamarin
ms.assetid: 944DFDB5-ADBA-4D6E-984C-5AEC19A1CC57
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/18/2017
ms.openlocfilehash: 7732419af22ab8bd37cbfbf828dc59e48841217f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="infoplist-reference"></a>Info.plist 참조

Info.Plist 키 작업에 대한 자세한 내용은 [보안 및 개인 정보 작업](~/ios/app-fundamentals/security-privacy.md) 가이드를 참조하세요. 

## <a name="location"></a>위치 

사용자 위치에 액세스하려면 Info.plist를 수정해야 합니다. 위치 데이터와 관련된 다음 키를 설정해야 합니다. 

* **NSLocationWhenInUseUsageDescription** - 사용자가 앱과 상호 작용하는 동안 사용자의 위치에 액세스하는 경우 
* **NSLocationAlwaysUsageDescription** - 앱이 백그라운드에서 사용자의 위치에 액세스하는 경우

## <a name="photos"></a>사진 

NSPhotoLibraryUsageDescription  

## <a name="contacts"></a>연락처 

NSContactsUsageDescription 

## <a name="calendar-data"></a>일정 데이터 
    
NSCalendarsUsageDescription 

## <a name="reminder-data"></a>미리 알림 데이터 
    
NSRemindersUsageDescription 

## <a name="bluetooth-peripherals"></a>Bluetooth 주변 장치 
    
NSBluetoothPeripheralUsageDescription 

## <a name="microphone"></a>마이크 

NSMicrophoneUsageDescription 

## <a name="camera"></a>카메라 
    
NSCameraUsageDescription 

## <a name="reading-healthkit"></a>HealthKit 읽기  

NSHealthShareUsageDescription 

NSHealthUpdateUsageDescription 

## <a name="background-modes"></a>백그라운드 모드 
    
UIBackgroundModes 

## <a name="homekit"></a>HomeKit 

NSHomeKitUsageDescription 


## <a name="related-links"></a>관련 링크

- [Apple 참조 가이드.](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW10)
