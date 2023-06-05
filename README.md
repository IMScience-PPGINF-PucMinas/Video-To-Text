Exploring attention mechanisms and hierarchical summarization in video captioning
=====
This is the Ph.D. Thesis project of Leonardo Vilela Cardoso for evaluating the impact of different attention mechanisms and feature selection with hierarchical keyframes selection.

## Abstract

A coherent description is an ultimate goal regarding video captioning via a couple of sentences because it might also affect the consistency and intelligibility of the generated results. In this context, a paragraph describing a video is affected by the activities used to both produce its specific narrative and provide some clues that can also assist in decreasing textual repetition. This work proposes a model, named Hierarchical timeaware Summarization with an Adaptive Transformer – HSAT, that uses a strategy to enhance the frame selection reducing the amount of information that needed to be processed along with attention mechanisms to enhance a memory-augmented transformer. This new approach increases the coherence among the generated sentences, assessing data importance (about the video segments) contained in the self-attention results and uses that to improve readability using only a small fraction of time spent by the other methods. The test results show the potential of this new approach as it provides higher coherence among the various video segments, decreasing the repetition in the generated sentences and improving the description diversity in the ActivityNet Captions dataset.

## Proposed Models
The problem of dense video captioning is related to the amount of similar information laid out sequentially with little or no variation. The relationship between the distribution of frames has a direct impact on the sentences generated and implies an increase or not in repetition. But, in many cases, part of the generated sentences can be repeated, diminishing the quality of the final result. That is more evident in an event-based description because, if there are different events (video segments) with high correlation, there is a high possibility that the same (or almost the same) sentence fragment appears more than once in the final result. Currently, some methods try to cope with that. However, this task is not trivial since it consists of evaluating the relationship between a text excerpt and all others being produced so that the described event is not repeated in the final result. Thus, some authors have studied the impact of applying methods based on deep learning in improving recognition of that kind of pattern. The use of learning techniques that make it possible to recurrently evaluate and relate the produced sentences has gained prominence because they generate sentences more consistent with the context by assessing the importance and contribution of each word to the final result.


### Enhanced Memory Transformer

To deal with coherence and reduce repetition, one possibility is the use of memory similar to one recurrent network.  Memory is used to control past information and newly learned information. This is the proposal for three new memory updater modules used to access each produced sentence's degree of relevance, present in Figure 1.

<p align="center">
  Figure 1 <br>
  <!--<img src="./results/ictai/mu-mart.jpeg" width="150">-->
  <img src="./results/ictai/mu-adaptive.jpeg" width="150">
  <img src="./results/ictai/mu-se.jpeg" width="150">
  <img src="./results/ictai/mu-adaptive-se.jpeg" width="150">
</p>

More information avalable on: [EMT](https://github.com/IMScience-PPGINF-PucMinas/EMT).
or on our paper: [EMT paper](https://ieeexplore.ieee.org/document/9643266)

### Adaptive Transformer

Another perspective is the analysis of the attention inside of the transformer. Thus, we propose to improve the self-attention module that exists within the main backbone of the transformer (just after the multi-head attention modules). So, we adopted additional attention blocks to emphasize the data generated by self-attention and cross-attention. The outline of our proposal, named Adaptive Transformer, is shown in Figure 2, which focuses on reducing repetition. The memory module is still an important part of the shared transformer module, and this is responsible to capture the long-term dependency of the sentences.

<p align="center">
  Figure 2 <br>
  <img src="./results/bigmm/adaptive-transformer.jpg" width="350">
</p>

More information avalable on: [Adaptive transformer](https://github.com/IMScience-PPGINF-PucMinas/Adaptive-Transformer)
or on our paper: [Adaptive transformer paper](https://ieeexplore.ieee.org/document/9999093)

### Hierarchical Time-Aware Keyframe Selection

Unlike the traditional approach that uses a sequential selection policy for frame selection, our proposed method chooses frames based on their similarity. It adopts a hierarchical graph-based summarization method to obtain the most valuable frames (as keyframes), present in Figure 3.

<p align="center">
  Figure 3 <br>
  <img src="./results/ijsc/outline.jpg" width="550">
</p>

A frame similarity graph is constructed and used by the video summarization approach. In this graph, each vertice represents a video frame. An edge between two vertices only exists if the difference between their time indexes is lower than a threshold  $\delta_t$. The edge weight represents the similarity value between the two frames associated with edge extremes. This process is shown in Figure 4.

<p align="center">
  Figure 4 <br>
  <img src="./results/ijsc/hvc.jpg" width="750">
</p>

The use of the threshold $\delta_t$ can be used to restrict the relationship between video frames far away and implies that our hierarchical graph-based summarization approach is time-aware (which also differs from many video summarization works in the literature.

More information avalable on: [HSAT Accepted on IJSC 2023](https://github.com/IMScience-PPGINF-PucMinas/HSAT)

## Results

To further promote the perception of the results obtained and their improvements, the inconsistencies of every approach had been highlighted to facilitate the visualization: (i) red/bold for cases of use of different pronouns from the ones in the GT result (or when they are misused); and (ii) blue/bold for the occurrences of a repeated sentence in the paragraph.

### Dataset
We apply the proposed methods to the ActivityNet Captions (ANC) dataset. This dataset contains 10,009 videos for training and 4,917 videos for validation. Videos used during the training step have a single reference paragraph, while validation videos have two reference paragraphs. Here, we follow the subdivision of this dataset to optimize the videos use and avoid overfitting. These subdivision, kept the training videos and divided the validation videos into two subsets, namely: ae-val with 2,460 videos for validation and ae-test with 2,457 videos for testing.

### Evaluation Metrics
The evaluation of sentences is a separate challenge, as there are several ways to write sentences, but with the same meaning, whether using synonyms or emphasizing information. This process is intuitive for humans. But there is not a specific approach for evaluating the video captioning task. So, what is usually done is the adaptation of machine translation metrics that are extended for this task such Bleu-4, CIDEr-D and Repetition-4.

### Video Captioning with Attention Applied on Memory Transformer
The paragraph generated by the use of different attentions methods on memory has a different focus than the description in the GT result, as shown in the Figure 5. But it is still possible to observe that the result produced preserves coherence and context with a low repeatability rate.

<p align="center">
  Figure 5 <br>
  <img src="./results/ictai/surf_cap.png" width="500">
</p>

### Video Captioning with Attention Used to Reinforce a first Self or Cross Attention

The qualitative results for the Adaptive Transformer are closer to the GT, it is possible to notice that the descriptions have coherence and fluidity. In addition, repeatability is reduced. The Figure 6 presents the result for the Adaptive transformer, and it's possible to notice that the result does not have pronoun errors or repetition and captures the continuity of the scene. But, the result does not have all descriptions compared to GT.
 
<p align="center">
  Figure 6 <br>
  <img src="./results/bigmm/yoga_cap.jpg" width="500">
</p>

### Video Captioning with Hierarchical Time-Aware Keyframe Selection

The results found for HSAT demonstrate that it presents an improvement related to the detection of video events. When compared to the sequential selection of frames, the amount of information that the method does not observe/process is large.

In some cases, when the sequential selection of frames is used, only the first one hundred frames with a rate of 2 FPS are used to represent the video. Thus, for A video with a length greater than 50 seconds, all information after the first 50 seconds is ignored. On the ANC dataset, the videos do not have the same length, the number of events varies from 2 to 6, and, in some cases, one video has more than two hundred seconds. Because of this, summarization appears as a better way to evaluate the content distributed in the entire video. Thus, the neglected information due to time limitations adopted in a sequential selection of frames does not exist with the hierarchical summarization approach. 

Despite that HSAT selects a relatively less number of frames (only 10), it is sufficient to cover all videos of the dataset (since the number of events in ANC dataset varies from more than 2).


Figure 7 and Figure 8 show the diversity of video content present in the dataset. The first shows the summarization result in a short video that has 25 seconds. Since it is a short video, the summarization process returns similar frames, however, with some minor variations in perspective. In video summarization, the amount of frames remains the same for all videos and the fluidity of the video is maintained. In addition, it is possible to correctly follow the actions over time without neglecting the video context.

<p align="center">
  Figure 7 <br>
  <img src="./results/ijsc/summarization-results-dog.jpg" width="800">
</p>

Figure 8 shows the frames selected as Keyframes for the HSAT method. While Figure 9 illustrates the selected frames  when a sequential selection (with time constraints) is made.
One can observe that in Figure 9 some content is not present at all. In contrast, HSAT manage to obtain a greater variety of video content making it easier to describe different moments of the video. In the sequential selection of frames, since that video has 80 seconds, it disregards any information that occurs in the final 30 seconds. In turn, HSAT uses hierarchical summarization to cover a greater variety of instants. In this way, HSAT only disregards very similar frames that are direct neighbors in time to include more distinct and meaningful frames for the video description. Due to the summarization process, the number of frames used can be reduced, generating results as significant as for techniques with large amounts of frames.

<p align="center">
  Figure 8 <br>
  <img src="./results/ijsc/summarization-results-salto.jpg" width="800">
</p>

<p align="center">
  Figure 9 <br>
  <img src="./results/ijsc/results-salto.jpg" width="800">
</p>

Figure 10 presents a qualitative comparison between the result obtained by the HSAT with the Adaptive Transformer. As one can see, the result is very discriminative and does not have many repetitions of terms. The results presented show very approximate descriptions. But, with some points described from another perspective. Thus, as HSAT uses features taken from different regions of the video and analyzes the importance of each frame in time, the modifications related to the description are due to the presence of points that may not be visualized in the same set of frames used to illustrate the video content. In this way, hierarchical summarization presents itself as a great candidate to improve the descriptions produced for the video captioning task.

<p align="center">
  Figure 10 <br>
  <img src="./results/ijsc/violin_cap_HSAT.jpg" width="500">
</p>

## Another Works and Implementation Details
[EMT](https://github.com/IMScience-PPGINF-PucMinas/EMT).

[Adaptive transformer](https://github.com/IMScience-PPGINF-PucMinas/Adaptive-Transformer)

[HSAT Accepted on IJSC 2023](https://github.com/IMScience-PPGINF-PucMinas/HSAT)

[HieTaSumm Submitted](soon)

## Citations
If you find this work useful for your research, please cite our papers:


```
@inproceedings{cardoso2022exploring,
  title={Exploring adaptive attention in memory transformer applied to coherent video paragraph captioning},
  author={Cardoso, Leonardo Vilela and Guimaraes, Silvio Jamil F and Patrocinio, Zenilton KG},
  booktitle={2022 IEEE Eighth International Conference on Multimedia Big Data (BigMM)},
  pages={37--44},
  year={2022},
  organization={IEEE}
}

@inproceedings{cardoso2021enhanced,
  title={Enhanced-Memory Transformer for Coherent Paragraph Video Captioning},
  author={Cardoso, Leonardo Vilela and Guimaraes, Silvio Jamil F and Patroc{\'\i}nio, Zenilton KG},
  booktitle={2021 IEEE 33rd International Conference on Tools with Artificial Intelligence (ICTAI)},
  pages={836--840},
  year={2021},
  organization={IEEE}
}
```

## Contact
"Leonardo Vilela Cardoso" with this e-mail: "leonardocardoso@pucminas.br"

"Zenilton Kleber Gonçalves do Patrocínio Junior" with this e-mail: "zenilton@pucminas.br"

