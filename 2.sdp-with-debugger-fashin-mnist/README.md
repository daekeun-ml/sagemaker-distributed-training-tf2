## Distributed training using Amazon SageMaker Distributed Data Parallel library and debugging using Amazon SageMaker Debugger

이 리포지토리에는 SageMaker의 분산 데이터 병렬(Distributed Data Parallel) 라이브러리를 사용하여 Amazon SageMaker에서 분산 훈련을 수행하고 Amazon SageMaker 디버거를 사용하여 디버깅하는 예제가 포함되어 있습니다. 훈련 스크립트는 디버거에 대한 스크립트 미변경 및 스크립트 변경 포함 시나리오를 모두 다룹니다.

### Overview

[Amazon SageMaker](https://aws.amazon.com/sagemaker/)는 모든 개발자 및 데이터 과학자에게 머신 러닝 (ML) 모델을 빠르게 구축, 훈련 및 배포할 수 있는 기능을 제공하는 완전 관리형 서비스입니다. SageMaker를 사용하면 기본 제공 알고리즘을 사용하고 자체 알고리즘 및 프레임워크를 가져올 수 있습니다. 그러한 프레임워크 중 하나가 TensorFlow 2.x입니다. 이 프레임워크로 분산 훈련을 수행할 때 SageMaker의 분산 데이터 병렬 또는 분산 모델 병렬 라이브러리를 사용할 수 있습니다. Amazon SageMaker Debugger는 훈련 작업을 실시간으로 디버그, 모니터링 및 프로파일링하여 비수렴(non-converging) 조건을 감지하고 병목 현상을 제거하여 리소스 사용률을 최적화하며 훈련 시간을 개선하고 머신 러닝 모델의 비용을 절감합니다.

이 예제에는 SageMaker에 최적화된 TensorFlow 2.x 컨테이너를 사용하여 SageMaker 분산 데이터 병렬 라이브러리를 사용하여 [Fashion MNIST 데이터셋](https://github.com/zalandoresearch/fashion-mnist)에 대한 분산 훈련을 수행하고 [SageMaker Debugger](https://docs.aws.amazon.com/sagemaker/latest/dg/train-debugger.html)를 사용하여 디버그하는 방법을 보여주는 Jupyter 노트북이 포함되어 있습니다.


## License

This library is licensed under the MIT-0 License. See the LICENSE file.
