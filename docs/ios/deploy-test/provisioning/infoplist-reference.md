---
title: Xamarin.iOS에 대한 Info.plist 참조
description: 이 문서에서는 Xamarin.iOS 응용 프로그램의 Info.plist 파일에서 설정할 수 있는 다양한 키/값 쌍을 설명합니다. 이러한 키는 앱에서 위치, 사진, 마이크 또는 카메라에 액세스와 같은 특정 작업을 수행하는 경우에 필요합니다.
ms.prod: xamarin
ms.assetid: 944DFDB5-ADBA-4D6E-984C-5AEC19A1CC57
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 01/18/2017
ms.openlocfilehash: 654eca1098f9486e0c41fd296b3f8d381ac7ea34
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105378"
---
# <a name="infoplist-reference-for-xamarinios"></a>Xamarin.iOS에 대한 Info.plist 참조

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
