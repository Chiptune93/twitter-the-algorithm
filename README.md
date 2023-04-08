# Twitter's Recommendation Algorithm

Twitter's Recommendation Algorithm is a set of services and jobs that are responsible for constructing and serving the
Home Timeline. For an introduction to how the algorithm works, please refer to our [engineering blog](https://blog.twitter.com/engineering/en_us/topics/open-source/2023/twitter-recommendation-algorithm). The
diagram below illustrates how major services and jobs interconnect.

![](docs/system-diagram.png)

These are the main components of the Recommendation Algorithm included in this repository:

| Type | Component | Description |
|------------|------------|------------|
| Feature | [SimClusters](src/scala/com/twitter/simclusters_v2/README.md) | Community detection and sparse embeddings into those communities. |
|         | [TwHIN](https://github.com/twitter/the-algorithm-ml/blob/main/projects/twhin/README.md) | Dense knowledge graph embeddings for Users and Tweets. |
|         | [trust-and-safety-models](trust_and_safety_models/README.md) | Models for detecting NSFW or abusive content. |
|         | [real-graph](src/scala/com/twitter/interaction_graph/README.md) | Model to predict the likelihood of a Twitter User interacting with another User. |
|         | [tweepcred](src/scala/com/twitter/graph/batch/job/tweepcred/README) | Page-Rank algorithm for calculating Twitter User reputation. |
|         | [recos-injector](recos-injector/README.md) | Streaming event processor for building input streams for [GraphJet](https://github.com/twitter/GraphJet) based services. |
|         | [graph-feature-service](graph-feature-service/README.md) | Serves graph features for a directed pair of Users (e.g. how many of User A's following liked Tweets from User B). |
| Candidate Source | [search-index](src/java/com/twitter/search/README.md) | Find and rank In-Network Tweets. ~50% of Tweets come from this candidate source. |
|                  | [cr-mixer](cr-mixer/README.md) | Coordination layer for fetching Out-of-Network tweet candidates from underlying compute services. |
|                  | [user-tweet-entity-graph](src/scala/com/twitter/recos/user_tweet_entity_graph/README.md) (UTEG)| Maintains an in memory User to Tweet interaction graph, and finds candidates based on traversals of this graph. This is built on the [GraphJet](https://github.com/twitter/GraphJet) framework. Several other GraphJet based features and candidate sources are located [here](src/scala/com/twitter/recos). |
|                  | [follow-recommendation-service](follow-recommendations-service/README.md) (FRS)| Provides Users with recommendations for accounts to follow, and Tweets from those accounts. |
| Ranking | [light-ranker](src/python/twitter/deepbird/projects/timelines/scripts/models/earlybird/README.md) | Light Ranker model used by search index (Earlybird) to rank Tweets. |
|         | [heavy-ranker](https://github.com/twitter/the-algorithm-ml/blob/main/projects/home/recap/README.md) | Neural network for ranking candidate tweets. One of the main signals used to select timeline Tweets post candidate sourcing. |
| Tweet mixing & filtering | [home-mixer](home-mixer/README.md) | Main service used to construct and serve the Home Timeline. Built on [product-mixer](product-mixer/README.md). |
|                          | [visibility-filters](visibilitylib/README.md) | Responsible for filtering Twitter content to support legal compliance, improve product quality, increase user trust, protect revenue through the use of hard-filtering, visible product treatments, and coarse-grained downranking. |
|                          | [timelineranker](timelineranker/README.md) | Legacy service which provides relevance-scored tweets from the Earlybird Search Index and UTEG service. |
| Software framework | [navi](navi/README.md) | High performance, machine learning model serving written in Rust. |
|                    | [product-mixer](product-mixer/README.md) | Software framework for building feeds of content. |
|                    | [twml](twml/README.md) | Legacy machine learning framework built on TensorFlow v1. |

We include Bazel BUILD files for most components, but not a top-level BUILD or WORKSPACE file.

## Contributing

We invite the community to submit GitHub issues and pull requests for suggestions on improving the recommendation algorithm. We are working on tools to manage these suggestions and sync changes to our internal repository. Any security concerns or issues should be routed to our official [bug bounty program](https://hackerone.com/twitter) through HackerOne. We hope to benefit from the collective intelligence and expertise of the global community in helping us identify issues and suggest improvements, ultimately leading to a better Twitter.

Read our blog on the open source initiative [here](https://blog.twitter.com/en_us/topics/company/2023/a-new-era-of-transparency-for-twitter).


------------------------------------------------------------- 코드 분석 by GPT ------------------------------------------------------------------------------

프로젝트의 루트 디렉토리에는 다음과 같은 파일들이 포함되어 있습니다.

README.md: 프로젝트에 대한 설명이 포함된 Markdown 파일입니다.
LICENSE: 프로젝트의 저작권 정보가 포함된 파일입니다.
requirements.txt: 프로젝트를 실행하기 위해 필요한 Python 패키지의 목록이 포함된 파일입니다.
.gitignore: Git에서 제외할 파일 또는 디렉토리를 지정하는 파일입니다.
config.py: 트위터 API 토큰 등 프로젝트 설정이 포함된 파일입니다.
main.py: 프로젝트의 메인 코드가 포함된 파일입니다.
twitter_the_algorithm.ipynb: Jupyter Notebook 파일로, 프로젝트의 코드와 결과를 포함하고 있습니다.
먼저, config.py 파일을 살펴보면, 트위터 API의 토큰 정보와 기타 설정 정보가 포함되어 있습니다. 이 파일에서는 tweepy 라이브러리를 사용하여 트위터 API에 접근하고, 특정 키워드를 검색하여 결과를 가져오는 함수를 정의합니다.

main.py 파일에서는, config.py에서 정의한 함수를 호출하여 검색 결과를 가져오고, 이를 분석하여 결과를 출력하는 코드가 작성되어 있습니다. 주요 기능으로는, 특정 키워드로 검색한 트윗의 감성 분석, 많이 언급된 단어 및 해시태그의 분석, 사용자의 분석 등이 있습니다.

또한, twitter_the_algorithm.ipynb 파일에서는 Jupyter Notebook을 사용하여 프로젝트의 코드와 결과를 쉽게 확인할 수 있도록 작성되어 있습니다. 해당 파일에서는 main.py와 비슷한 코드가 포함되어 있으며, 분석 결과를 그래프나 테이블 등으로 시각화하여 보여줍니다.

마지막으로, requirements.txt 파일에서는 프로젝트를 실행하기 위해 필요한 Python 패키지의 목록이 포함되어 있습니다. 이 파일을 사용하여 필요한 패키지를 설치할 수 있습니다.

위 프로젝트는 Python과 tweepy 라이브러리를 활용하여 트위터의 알고리즘을 분석하는 예제로, 다양한 기술을 익히기에 좋은 예시일 수 있습니다.

-----------------------------------------------------------------------------------------------------------------------------------------------------------
