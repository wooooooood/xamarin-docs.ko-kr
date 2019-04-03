---
title: Xamarin.iOS를 사용 하 여 자연 언어 프레임 워크를 사용 하 여
description: 이 문서에서는 자연 언어 프레임 워크를 설명 합니다. IOS 12에에서 도입 된, 자연 언어 프레임 워크는 언어 인식, 음성의 부분 식별 및 명명 된 엔터티 인식에 사용할 기본 iOS API.
ms.prod: xamarin
ms.assetid: 126C8764-F873-4EB9-98A3-D82AB5689111
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/20/2018
ms.openlocfilehash: 41f629739b06431a9b20548f61111bc31e911abb
ms.sourcegitcommit: 495680e74c72e7c570e68cde95d3d3643b1fcc8a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58870042"
---
# <a name="using-the-natural-language-framework-with-xamarinios"></a>Xamarin.iOS를 사용 하 여 자연 언어 프레임 워크를 사용 하 여

자연 언어 프레임 워크를 iOS 12에에서 도입 된, 장치에 자연어 처리를 사용 하도록 설정 합니다. 언어 인식, 토큰화 및 태그 지정을 지원합니다. 토큰화에 해당 구성 요소 단어, 문장 또는 단락; 텍스트를 분 품사, 사람, 장소 및 조직의 식별 태그를 지정 합니다.

자연 언어 프레임 워크를 분류 하 고 특수화 된 컨텍스트에서 텍스트 태그를 사용자 지정 핵심 ML 모델을 사용할 수도 있습니다.

합니다 [NSLinguisticTagger](xref:Foundation.NSLinguisticTagger) 클래스는 계속 사용할 수 있습니다. 그러나 자연 언어 프레임 워크는 자연어 처리를 위해 사용 하는 기본 메커니즘입니다.

## <a name="sample-app-xamarinnl"></a>샘플 앱: XamarinNL

Xamarin.iOS를 사용 하 여 자연 언어 프레임 워크를 사용 하는 방법에 알아보려면 살펴보겠습니다 합니다 [XamarinNL 샘플 앱](https://developer.xamarin.com/samples/monotouch/iOS12/XamarinNL)합니다.
이 샘플 앱에 자연 언어 프레임 워크를 사용 하는 방법을 보여 줍니다.

- [언어 인식](#recognizing-languages)합니다.
- [단어 및 문장으로 텍스트 토큰화](#tokenizing-text-into-words-sentences-and-paragraphs)합니다.
- [명명 된 엔터티 및 품사 태그](#tagging-named-entities-and-parts-of-speech)합니다.

## <a name="recognizing-languages"></a>언어 인식

합니다 **인식기** 샘플 앱의 탭에는 사용 하는 방법을 보여 줍니다.는 [`NLLanguageRecognizer`](xref:NaturalLanguage.NLLanguageRecognizer)
텍스트 블록에 대 한 언어를 확인 합니다.

> [!NOTE]
> 언어 인식에는 텍스트 분류의 특정 형식입니다. 자연 언어 프레임 워크는 또한 개발자가 제공한 핵심 ML 모델을 통해 사용자 지정 텍스트 분류를 지원합니다. 자세한 내용은 잠시 살펴 합니다 [자연 언어 프레임 워크 소개](https://developer.apple.com/videos/play/wwdc2018/713/) WWDC 2018에서 세션입니다.

### <a name="dominant-language"></a>주요 언어

탭의 **언어** 사용자 입력의 주 언어를 식별 하는 단추입니다.

`HandleDetermineLanguageButtonTap` 메서드는 `LanguageRecognizerViewController` 사용 하는 [`GetDominantLanguage`](xref:NaturalLanguage.NLLanguageRecognizer.GetDominantLanguage*)
메서드는 `NLLanguageRecognizer` 인출에는 [`NLLanguage`](xref:NaturalLanguage.NLLanguage)
텍스트에서 찾은 기본 언어:

```csharp
partial void HandleDetermineLanguageButtonTap(UIButton sender)
{
    UserInput.ResignFirstResponder();
    if (!String.IsNullOrWhiteSpace(UserInput.Text))
    {
        NLLanguage lang = NLLanguageRecognizer.GetDominantLanguage(UserInput.Text);
        DominantLanguageLabel.Text = lang.ToString();
    }
}
```

### <a name="language-probabilities"></a>언어 확률

탭의 **언어 확률** 단추 사용자 입력에 대 한 언어 가설의 목록을 가져올 수 있습니다.

합니다 `HandleLanguageProbabilitiesButtonTap` 메서드를 `LanguageRecognizerViewController` 클래스를 인스턴스화하는 `NLLanguageRecognizer` 하 라고 요청 [`Process`](xref:NaturalLanguage.NLLanguageRecognizer.Process*)
사용자의 텍스트입니다. 그런 다음 언어 인식기의 호출 [`GetNativeLanguageHypotheses`](xref:NaturalLanguage.NLLanguageRecognizer.GetNativeLanguageHypotheses*)
언어와 연관 된 확률의 사전 인출 하는 메서드. `LanguageRecognizerTableViewController` 이러한 언어 및 확률 클래스 렌더링 합니다.

```csharp
partial void HandleLanguageProbabilitiesButtonTap(UIButton sender)
{
    UserInput.ResignFirstResponder();
    if (!String.IsNullOrWhiteSpace(UserInput.Text))
    {
        var recognizer = new NLLanguageRecognizer();
        recognizer.Process(UserInput.Text);
        NSDictionary<NSString, NSNumber> probabilities = recognizer.GetNativeLanguageHypotheses(10);
        PerformSegue(ShowLanguageProbabilitiesSegue, this);
    }
}
```

잠재적인 `NLLanguage` 값 포함:

- `Amharic`
- `Arabic`
- `Armenian`
- `Bengali`
- `Bulgarian`
- `Burmese`
- `Catalan`
- `Cherokee`
- `Croatian`
- `Czech`
- `Danish`
- `Dutch`
- `English`
- `Finnish`
- `French`
- `Georgian`
- `German`
- `Greek`
- `Gujarati`
- `Hebrew`
- `Hindi`
- `Hungarian`
- `Icelandic`
- `Indonesian`
- `Italian`
- `Japanese`
- `Kannada`
- `Khmer`
- `Korean`
- `Lao`
- `Malay`
- `Malayalam`
- `Marathi`
- `Mongolian`
- `Norwegian`
- `Oriya`
- `Persian`
- `Polish`
- `Portuguese`
- `Punjabi`
- `Romanian`
- `Russian`
- `SimplifiedChinese`
- `Sinhalese`
- `Slovak`
- `Spanish`
- `Swedish`
- `Tamil`
- `Telugu`
- `Thai`
- `Tibetan`
- `TraditionalChinese`
- `Turkish`
- `Ukrainian`
- `Undetermined`
- `Urdu`
- `Vietnamese`

지원 되는 언어의 전체 목록은 제품은 부분을 [`NLLanguage`](xref:NaturalLanguage.NLLanguage)
enum API 설명서입니다.

## <a name="tokenizing-text-into-words-sentences-and-paragraphs"></a>단어, 문장, 단락에 텍스트를 토큰화

합니다 **토크 나이저** 샘플 앱의 탭 텍스트 블록의 해당 구성 요소 단어로 분리 하는 방법에 설명 이나 사용 하 여 문장는 [ `NLTokenizer` ](xref:NaturalLanguage.NLTokenizer)합니다.

탭의 **단어** 또는 **문장을** 단추 토큰 목록을 가져올 수 있습니다. 각 토큰 단어나 문장을 원래 텍스트에서와 연결 됩니다.

`ShowTokens` 호출 하 여 사용자의 입력을 토큰으로 분할 합니다 [`GetTokens`](xref:NaturalLanguage.NLTokenizer.GetTokens*)
메서드는 `NLTokenizer`합니다. 이 메서드는 배열을 반환합니다 [`NSValue`](xref:Foundation.NSValue)
개체에 각 배치를 `NSRange` 원본 텍스트의 토큰에 해당 하는 값입니다.

```csharp
void ShowTokens(NLTokenUnit unit)
{
    if (!String.IsNullOrWhiteSpace(UserInput.Text))
    {
        var tokenizer = new NLTokenizer(unit);
        tokenizer.String = UserInput.Text;
        var range = new NSRange(0, UserInput.Text.Length);
        NSValue[] tokens = tokenizer.GetTokens(range);
        PerformSegue(ShowTokensSegue, this);
    }
}
```

`LanguageTokenizerTableViewController` 각 테이블 셀에 단일 토큰을 렌더링합니다. 추출를 `NSRange` 토큰에서 `NSValue`, 원래 텍스트에서 해당 문자열을 찾습니다 및 레이블을 테이블 뷰 셀에 설정 합니다.

```csharp
public override UITableViewCell GetCell(UITableView tableView, NSIndexPath indexPath)
{
    var cell = TableView.DequeueReusableCell(TokenCell);
    NSRange range = Tokens[indexPath.Row].RangeValue;
    cell.TextLabel.Text = Text.Substring((int)range.Location, (int)range.Length);
    return cell;
}
```

## <a name="tagging-named-entities-and-parts-of-speech"></a>명명 된 엔터티 및 품사 태그 지정

합니다 **태거** XamarinNL 샘플 앱의 탭을 사용 하는 방법에 설명 합니다 [`NLTagger`](xref:NaturalLanguage.NLTagger)
입력된 문자열의 토큰을 사용 하 여 범주를 연결 하는 클래스입니다.
자연 언어 프레임 워크에는 사람, 장소, 조직 및 파트의 음성 인식에 대 한 기본 제공 지원이 포함 됩니다.

> [!NOTE]
> 자연 언어 프레임 워크는 또한 개발자가 제공한 핵심 ML 모델을 통해 사용자 지정 태그 지정 체계를 지원합니다. 자세한 내용은 잠시 살펴 합니다 [자연 언어 프레임 워크 소개](https://developer.apple.com/videos/play/wwdc2018/713/) WWDC 2018에서 세션입니다.

탭의 **명명 된 엔터티** 하거나 **품사** 인출 하는 단추:

- 배열을 `NSValue` 개체에 각 배치를 `NSRange` 원본 텍스트의 토큰에 대 한 합니다.
- 배열을 [ `NLTag` ](xref:NaturalLanguage.NLTag) 값-에 대 한 범주를 `NSValue` 동일한 배열 인덱스에서 토큰입니다.

`LanguageTaggerViewController`, `HandlePartsOfSpeechButtonTap` 및 `HandleNamedEntitiesButtonTap` 호출할 때마다 `ShowTags`함께 전달는 [ `NLTagScheme` ](xref:NaturalLanguage.NLTagScheme) 하거나 – `NLTagScheme.LexicalClass` (품사)에 대 한 또는 `NLTagScheme.NameType` (에 대 한 명명 된 엔터티).

`ShowTags` 만듭니다는 `NLTagger`의 배열을 사용 하 여 인스턴스화한 `NLTagScheme` 하는 것은 쿼리할 수에 대 한 형식 (만 전달에이 예제의 `NLTagScheme` 값). 사용 하 여는 [`GetTags`](xref:NaturalLanguage.NLTagger.GetTags*)
메서드는 `NLTagger` 텍스트 사용자 입력에 관련 태그를 확인 하려면.

```csharp
void ShowTags(NLTagScheme tagScheme)
{
    if (!String.IsNullOrWhiteSpace(UserInput.Text))
    {
        var tagger = new NLTagger(new NLTagScheme[] { tagScheme });
        var range = new NSRange(0, UserInput.Text.Length);
        tagger.String = UserInput.Text;

        NLTag[] tags = tagger.GetTags(range, NLTokenUnit.Word, tagScheme, NLTaggerOptions.OmitWhitespace, out NSValue[] ranges);
        NSValue[] tokenRanges = ranges;
        detailViewTitle = tagScheme == NLTagScheme.NameType ? "Named Entities" : "Parts of Speech";

        PerformSegue(ShowEntitiesSegue, this);
    }
}
```

태그는 다음 테이블에 표시 된 `LanguageTaggerTableViewController`합니다.

잠재적인 `NLTag` 값 포함:

- `Adjective`
- `Adverb`
- `Classifier`
- `CloseParenthesis`
- `CloseQuote`
- `Conjunction`
- `Dash`
- `Determiner`
- `Idiom`
- `Interjection`
- `Noun`
- `Number`
- `OpenParenthesis`
- `OpenQuote`
- `OrganizationName`
- `Other`
- `OtherPunctuation`
- `OtherWhitespace`
- `OtherWord`
- `ParagraphBreak`
- `Particle`
- `PersonalName`
- `PlaceName`
- `Preposition`
- `Pronoun`
- `Punctuation`
- `SentenceTerminator`
- `Verb`
- `Whitespace`
- `Word`
- `WordJoiner`

지원 되는 태그의 전체 목록은 제품은 부분을 [`NLTag`](xref:NaturalLanguage.NLTag)
enum API 설명서입니다.

## <a name="related-links"></a>관련 링크

- [샘플 앱 – XamarinNL](https://developer.xamarin.com/samples/monotouch/iOS12/XamarinNL)
- [자연 언어 프레임 워크를 소개합니다.](https://developer.apple.com/videos/play/wwdc2018/713/)
- [자연 언어 (Apple)](https://developer.apple.com/documentation/naturallanguage?language=objc)
