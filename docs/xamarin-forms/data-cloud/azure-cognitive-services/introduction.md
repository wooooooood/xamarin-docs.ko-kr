---
title: Xamarin.ios 및 Azure Cognitive Services 소개
description: 이 문서에서는 일부 Microsoft 인식 서비스 Api를 호출 하는 방법을 보여 주는 예제 응용 프로그램에 대해 소개 합니다.
ms.prod: xamarin
ms.assetid: 74121ADB-1322-4C1E-A103-F37257BC7CB0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 52774b387644b14e3d4612dffa6d3c3b28a37f25
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "68652306"
---
# <a name="xamarinforms-and-azure-cognitive-services-introduction"></a>Xamarin.ios 및 Azure Cognitive Services 소개

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

_Microsoft Cognitive Services는 개발자가 얼굴 인식, 음성 인식 및 언어 이해와 같은 기능을 추가 하 여 응용 프로그램을 보다 지능적으로 만드는 데 사용할 수 있는 Api, Sdk 및 서비스 집합입니다. 이 문서에서는 일부 Microsoft 인식 서비스 Api를 호출 하는 방법을 보여 주는 예제 응용 프로그램에 대해 소개 합니다._

## <a name="overview"></a>개요

함께 제공 되는 샘플은 다음과 같은 기능을 제공 하는 할 일 목록 응용 프로그램입니다.

- 작업 목록을 봅니다.
- 소프트 키보드를 통해 또는 Microsoft Speech API에서 음성 인식을 수행 하 여 작업을 추가 및 편집 합니다. 음성 인식을 수행 하는 방법에 대 한 자세한 내용은 [Microsoft Speech API 사용 하 여 음성 인식](speech-recognition.md)을 참조 하세요.
- Bing Spell Check API를 사용 하 여 맞춤법 검사 작업을 합니다. 자세한 내용은 [Bing Spell Check API 사용 하 여 맞춤법 검사](spell-check.md)를 참조 하세요.
- Translator API를 사용 하 여 작업을 영어에서 독일어로 변환 합니다. 자세한 내용은 [TRANSLATOR API를 사용 하 여 텍스트 번역](text-translation.md)을 참조 하세요.
- 작업을 삭제 합니다.
- 작업 상태를 ' 완료 '로 설정 합니다.
- Face API를 사용 하 여 응용 프로그램을 emotion 인식으로 평가 합니다. 자세한 내용은 [Face API를 사용 하는 Emotion 인식](emotion-recognition.md)을 참조 하세요.

태스크는 로컬 SQLite 데이터베이스에 저장 됩니다. 로컬 SQLite 데이터베이스를 사용 하는 방법에 대 한 자세한 내용은 [로컬 데이터베이스 작업](~/xamarin-forms/data-cloud/data/databases.md)을 참조 하세요.

응용 프로그램이 시작 될 때 `TodoListPage` 표시 됩니다. 이 페이지는 로컬 데이터베이스에 저장 된 모든 작업 목록을 표시 하 고 사용자가 새 작업을 만들거나 응용 프로그램의 등급을 지정할 수 있도록 합니다.

![](introduction-images/sample-application-1.png "TodoListPage")

@No__t_2를 탐색 하는 *+* 단추를 클릭 하 여 새 항목을 만들 수 있습니다. 작업을 선택 하 여이 페이지를 탐색할 수도 있습니다.

![](introduction-images/sample-application-2.png "TodoItemPage")

@No__t_0를 사용 하 여 작업을 만들고, 편집 하 고, 맞춤법을 검사 하 고, 번역 하 고, 저장 하 고, 삭제할 수 있습니다. 음성 인식을 사용 하 여 작업을 만들거나 편집할 수 있습니다. 이렇게 하려면 마이크 단추를 눌러 기록을 시작 하 고, 동일한 단추를 두 번 눌러 기록을 중지할 수 있습니다. 그러면 기록을 중지할 수 있으며,이는 기록을 Bing Speech 인식 API로 보냅니다.

@No__t_0에서 smilies 단추를 클릭 하면 얼굴 식 이미지에 대해 emotion 인식 기능을 수행 하는 데 사용 되는 `RateAppPage` 이동 합니다.

![](introduction-images/sample-application-3.png "RateAppPage")

@No__t_0를 사용 하면 표시 되는 반환 된 emotion를 사용 하 여 Face API에 제출 되는 얼굴 사진을 가져올 수 있습니다.

## <a name="understand-the-application-anatomy"></a>응용 프로그램 분석 이해

샘플 응용 프로그램에 대 한 공유 코드 프로젝트는 다음과 같은 5 개의 기본 폴더로 구성 됩니다.

|폴더|용도|
|--- |--- |
|모델|응용 프로그램에 대 한 데이터 모델 클래스를 포함 합니다. 여기에는 응용 프로그램에서 사용 하는 데이터의 단일 항목을 모델링 하는 `TodoItem` 클래스가 포함 됩니다. 이 폴더에는 여러 Microsoft 인식 서비스 Api에서 반환 된 JSON 응답을 모델링 하는 데 사용 되는 클래스도 포함 되어 있습니다.|
|리포지토리에서|데이터베이스 작업을 수행 하는 데 사용 되는 `ITodoItemRepository` 인터페이스 및 `TodoItemRepository` 클래스를 포함 합니다.|
|서비스|플랫폼 프로젝트의 인터페이스를 구현 하는 클래스를 찾기 위해 `DependencyService` 클래스에서 사용 하는 인터페이스와 함께 다양 한 Microsoft 인지 서비스 Api에 액세스 하는 데 사용 되는 인터페이스와 클래스를 포함 합니다.|
|유틸리티|@No__t_1 클래스에서 9 분 마다 JWT 액세스 토큰을 갱신 하는 데 사용 하는 `Timer` 클래스를 포함 합니다.|
|보기|응용 프로그램에 대 한 페이지를 포함 합니다.|

공유 코드 프로젝트에는 몇 가지 중요 한 파일도 포함 되어 있습니다.

|파일|용도|
|--- |--- |
|Constants.cs|호출 되는 Microsoft 인식 서비스 Api에 대 한 API 키 및 끝점을 지정 하는 `Constants` 클래스입니다. API 키 상수를 업데이트 하려면 다른 인지 서비스 Api에 액세스 해야 합니다.|
|App.xaml.cs|@No__t_0 클래스는 각 플랫폼에서 응용 프로그램에 의해 표시 되는 첫 페이지와 데이터베이스 작업을 호출 하는 데 사용 되는 `TodoManager` 클래스를 모두 인스턴스화하는 역할을 합니다.|

### <a name="nuget-packages"></a>NuGet 패키지

샘플 응용 프로그램은 다음 NuGet 패키지를 사용 합니다.

- `Newtonsoft.Json` – .NET 용 JSON 프레임 워크를 제공 합니다.
- `PCLStorage` – 플랫폼 간 로컬 파일 IO Api 집합을 제공 합니다.
- `sqlite-net-pcl` – SQLite 데이터베이스 저장소를 제공 합니다.
- `Xam.Plugin.Media` – 플랫폼 간 사진을 만들고 선택 하는 Api를 제공 합니다.

또한 이러한 NuGet 패키지는 자체 종속성을 설치 합니다.

### <a name="model-the-data"></a>데이터 모델링

이 샘플 응용 프로그램에서는 `TodoItem` 클래스를 사용 하 여 로컬 SQLite 데이터베이스에 표시 되 고 저장 되는 데이터를 모델링 합니다. 다음 코드 예제는 `TodoItem` 클래스를 보여줍니다.

```csharp
public class TodoItem
{
  [PrimaryKey, AutoIncrement]
  public int ID { get; set; }
  public string Name { get; set; }
  public bool Done { get; set; }
}
```

@No__t_0 속성은 각 `TodoItem` 인스턴스를 고유 하 게 식별 하는 데 사용 되며, 속성을 데이터베이스의 자동 증분 기본 키로 설정 하는 SQLite 특성으로 데코 레이트 됩니다.

### <a name="invoke-database-operations"></a>데이터베이스 작업 호출

@No__t_0 클래스는 데이터베이스 작업을 구현 하 고 클래스의 인스턴스는 `App.TodoManager` 속성을 통해 액세스할 수 있습니다. @No__t_0 클래스는 다음과 같은 메서드를 제공 하 여 데이터베이스 작업을 호출 합니다.

- **Getallitems async** – 로컬 SQLite 데이터베이스에서 모든 항목을 검색 합니다.
- **Getitemasync** – 로컬 SQLite 데이터베이스에서 지정 된 항목을 검색 합니다.
- **Saveitemasync** – 로컬 SQLite 데이터베이스에서 항목을 만들거나 업데이트 합니다.
- **Deleteitemasync** – 로컬 SQLite 데이터베이스에서 지정 된 항목을 삭제 합니다.

### <a name="platform-project-implementations"></a>플랫폼 프로젝트 구현

공유 코드 프로젝트의 `Services` 폴더에는 `DependencyService` 클래스에서 플랫폼 프로젝트의 인터페이스를 구현 하는 클래스를 찾기 위해 사용 하는 `IFileHelper` 및 `IAudioRecorderService` 인터페이스가 포함 되어 있습니다.

@No__t_0 인터페이스는 각 플랫폼 프로젝트에서 `FileHelper` 클래스에 의해 구현 됩니다. 이 클래스는 SQLite 데이터베이스를 저장 하기 위한 로컬 파일 경로를 반환 하는 단일 메서드인 `GetLocalFilePath`으로 구성 됩니다.

@No__t_0 인터페이스는 각 플랫폼 프로젝트에서 `AudioRecorderService` 클래스에 의해 구현 됩니다. 이 클래스는 플랫폼 Api를 사용 하 여 장치의 마이크에서 오디오를 기록 하 고 wav 파일로 저장 하는 `StartRecording`, `StopRecording` 및 지원 메서드로 구성 됩니다. IOS에서 `AudioRecorderService`는 `AVFoundation` API를 사용 하 여 오디오를 기록 합니다. Android에서 `AudioRecordService`는 `AudioRecord` API를 사용 하 여 오디오를 기록 합니다. UWP (유니버설 Windows 플랫폼)에서 `AudioRecorderService`는 `AudioGraph` API를 사용 하 여 오디오를 기록 합니다.

### <a name="invoke-cognitive-services"></a>인식 서비스 호출

샘플 응용 프로그램은 다음 Microsoft Cognitive Services를 호출 합니다.

- Microsoft Speech API. 자세한 내용은 [Microsoft Speech API를 사용한 음성 인식](speech-recognition.md)을 참조 하세요.
- Bing Spell Check API. 자세한 내용은 [Bing Spell Check API 사용 하 여 맞춤법 검사](spell-check.md)를 참조 하세요.
- API를 변환 합니다. 자세한 내용은 [TRANSLATOR API를 사용 하 여 텍스트 번역](text-translation.md)을 참조 하세요.
- Face API. 자세한 내용은 [Face API를 사용 하는 Emotion 인식](emotion-recognition.md)을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [Microsoft Cognitive Services 설명서](https://www.microsoft.com/cognitive-services/documentation)
- [Todo Cognitive Services (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
