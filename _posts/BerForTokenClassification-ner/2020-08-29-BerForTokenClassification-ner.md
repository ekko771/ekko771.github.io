---
title: How to Fine-Tune BERT for Named Entity Recognition
date: 2020-08-29 13:00:00 +08:00
tags: [Natural Language Processing, Transformers, Named Entity Recognition]
description: This article is Fine-Tune BERT for Named Entity Recognition.
---

# How to Fine-Tune BERT for Named Entity Recognition


## Name Entity 是什麼?

所謂的Name Entity，是自然語言處理中的一項任務，也跟資訊檢索中的資訊擷取議題有關。在全文文件中，常有人名、地名、機構名等詞彙出現，以及關於時間、金錢等不同格式數據的表達，這些詞彙經常不會出現在既有的詞庫中，因此需要特別的標註，以便擷取及應用。以此句為例：「2006年10月9日，Google宣布以16.5億美元的股票收購YouTube網站。」，專有名詞標記需將：「2006年10月9日」標註成時間名詞並轉換成日期，「Google」、「YouTube」標註為組織名，「16.5億美元」標註為金錢數量。

## 如何訂定Name Entity?

我們通常在訂定Name Entity的時候，通常會從有兩個點依序下去思考:
這個domain有哪些可能的task。
那每個task可能會需要知道的Name Entity。
基本上基於這兩個點，我們才不會定義出不相關的或是未來可能還有該辨識的但是當初沒有定義到。 我們可以看出來Name Entity呼應到了開頭提到的，在特定應用場景下，跟特定task相關的元素。
我想有些朋友讀到這邊會產生出另外一個疑問，假如Name Entity都是domain specific，那麼為什麼會有一些general entity呢?
其實是這樣子的，所謂的general entity也是基於某一個domain (一般群眾在一般生活環境)去訂定的。這邊列舉一些general entity:

* Stanford typical 3 classes:  
    LOCATION, ORGANIZATION, PERSON
* Stanford typical 7 classes:  
    DATE, LOCATION, MONEY, ORGANIZATION, PERCENT, PERSON, TIME
* Stanford typical 4 classes:  
    LOCATION, MISC, ORGANIZATION, PERSON

但假如我在不同的應用中，舉例來說刑事案件中，對於PLACE來說，我可能還會有不同的Entity像是命案現場、發現現場等，到底是哪個entity還是要再根據上下文去做判斷。

## Name Entity Recognition Method
* 基於規則和字典的方法:  
    <br/>
    此方法是基於專家知識制定出模板及規則，並透過統計資訊、關鍵字、標點符號、位置詞等等眾多詞彙當作特徵進行字串匹配為主要的手段，這類系統大多依賴於知識庫和詞典的建立。  
    <br/>
    例如使用POS Pattern來進行Entity查找，它就是藉由統計Entity 底下某種POS Pattern出現的機率來設計，例如動詞接名詞的Pattern就定義為事件。 ex 喝咖啡、看電影、吹乒乓球。  
    <br/>
* 基於統計機器學習的方法:  
    <br/>
    在基於機器學習的方法中，NER被當作序列標註問題。利用大規模語料來學習出標註模型，從而對句子的各個位置進行標註。NER 任務中的常用模型包括生成式模型HMM、判別式模型CRF等。條件隨機場（Conditional Random Field，CRF）是NER目前的主流模型。  
    <br/>
    HMM主要就是在計算狀態之間轉移機率，因句子本來就是一個具有順序的資料，也就是說詞跟詞之前的轉換，可以視為一個狀態轉到下一個狀態的架構，我們主要就是在計算狀態跟狀態轉換的機率。  
    <br/> 
    條件隨機場（CRF） 的目標函數不僅考慮輸入的狀態特徵函數，而且還包含了標籤轉移特徵函數。在訓練時可以使用SGD學習模型參數。在已知模型時，給輸入序列求預測輸出序列即求使目標函數最大化的最優序列，是一個動態規劃問題，可以使用維特比算法進行解碼。CRF的優點在於其爲一個位置進行標註的過程中可以利用豐富的內部及上下文特徵信息。  
    <br/>
* 基於深度學習的方法:  
    <br/>
    主要就是將token從離散one-hot表示映射到低維空間中成為稠密的embedding，隨後將句子的embedding序列輸入到深度學習中對於處理序列性資料有很好效果的RNN中，用神經網絡自動提取特徵，Softmax來預測每個token的標籤。這種方法使得模型的訓練成為一個端到端的過程，而非傳統的pipeline，不依賴於特徵工程，是一種數據驅動的方法，但網絡種類繁多、對參數設置依賴大，模型可解釋性差。此外，這種方法的一個缺點是對每個token打標籤的過程是獨立的進行，不能直接利用上文已經預測的標籤（只能靠隱含狀態傳遞上文信息），進而導致預測出的標籤序列可能是無效的，例如標籤I-PER後面是不可能緊跟著B-PER的，但Softmax不會利用到這個信息。所以學界提出了DL-CRF模型做序列標註。在神經網絡的輸出層接入CRF層(重點是利用標籤轉移機率)來做句子級別的標籤預測，使得標註過程不再是對各個token獨立分類。  
    <br/>
    而現在最流行的便是biLSTM-CRF模型主要由Embedding層（主要有詞向量，字向量以及一些額外特徵），雙向LSTM層(不只可以學習由左到右的信息，甚至由右到左的信息都學的到)，以及最後的CRF層構成。實驗結果表明biLSTM-CRF已經達到或者超過了基於豐富特徵的CRF模型，成為目前基於深度學習的NER方法中的最主流模型。在特徵方面，該模型繼承了深度學習方法的優勢，無需特徵工程，使用詞向量以及字符向量就可以達到很好的效果，如果有高質量的詞典特徵，能夠進一步獲得提高。   
    ![bilstm-crf](https://raw.githubusercontent.com/ekko771/ekko771.github.io/master/_posts/BerForTokenClassification-ner/bilstm-crf.jpg)  
    <br/>
* 基於注意力機制和半監督式學習方法:  
    <br/>
    主要就是因為隨着Bert語言模型在NLP領域橫掃了11項任務的最優結果，將其在命名實體識別中Fine-tune必然成爲趨勢。它主要是使用bert模型替換了原來網絡的word2vec部分，從而構成Embedding層，同樣使用雙向LSTM層以及最後的CRF層來完成序列預測，簡而言之可以理解成bert同樣也是產生embedding的工具，只不過這個embedding比Word2vec的embedding要厲害，因為bert的embedding是可以先經由其它任務去pre-train它的模型。  
    <br/>
    ![bilstm-crf](https://raw.githubusercontent.com/ekko771/ekko771.github.io/master/_posts/BerForTokenClassification-ner/bert-ner.jpg) 
      
