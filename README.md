# Amazon SageMaker Distributed Training Hands-on Lab - TensorFlow 2.x

## Introduction
본 핸즈온은 TensorFlow 2.x에서 SageMaker 분산 훈련을 수행하며, AWS 공식 예제들을 보완 및 한글화하였습니다.

### Prerequisite (Recommended)
- [SageMaker 오버뷰(50분)](https://www.youtube.com/watch?v=jF2BN98KBlg)
- [SageMaker 데모(60분)](https://www.youtube.com/watch?v=miIVGlq6OUk)
- [TensorFlow 2.x BYOS(Bring Your Own Script) and BYOC(Bring Your Own Container) Hands-on Lab](https://github.com/daekeun-ml/sagemaker-byos-byoc)

### Caution 
- SageMaker Distributed Data Parallelism 핸즈온은 `ml.p3.16xlarge, ml.p3dn.24xlarge, ml.p4d.24xlarge` 인스턴스만 지원하며, 이머전데이 및 워크샵은 반드시 `ml.p3.16xlarge`로 수행하셔야 합니다.

## Hands-on Labs
- [1.SageMaker Debugger Profiler](1.profiling)
- [2.SageMaker Distributed Data Parallelism w/ Debugger Profiler - Fashion MNIST dataset](2.sdp-with-debugger-fashion-mnist)

## References
- TensorFlow Profiling: https://github.com/aws/amazon-sagemaker-examples/tree/master/sagemaker-debugger/tensorflow_profiling 
- SageMaker Distributed Data Parallelism w/ Debugger Profiler: https://github.com/aws-samples/amazon-sagemaker-dist-data-parallel-with-debugger

## License Summary
이 샘플 코드는 MIT-0 라이센스에 따라 제공됩니다. LICENSE 파일을 참조하십시오.