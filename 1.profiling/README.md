# Amazon SageMaker Debugger & Debugger Profiling

## Why Amazon SageMaker Debugger? 
분산 훈련 시 많은 리소스를 활용하다 보면 학습 성능 외에도 모델에 활용되는 리소스가 충분히 활용되고 있는지 검토해야 합니다. 기본적으로 모델링 코드에서 로그를 기록하거나, `nvidia-smi`로 GPU 메모리 사용률을 확인할 수 있지만, 이렇게 해서 얻게 되는 정보의 양은 제한적입니다.

이 때, Amazon SageMaker Debugger를 사용하면 모델 성능에 대한 모니터링부터 훈련에서 발생하는 이상 상태를 탐지할 수 있습니다. 이를 위해 Tensor 정보를 수집하고, 훈련 클러스터의 리소스 사용률을 모니터링하여 분산 훈련에 대한 리소스 상태와 분산 훈련의 속도에 영향을 줄 수 있는 리소스 bottleneck 여부 등을 판단할 수 있습니다.

## Amazon SageMaker Debugger

### Overview
Amazon SageMaker Debugger는 훈련 코드를 별도로 수정하지 않고 실시간으로 Tensor 데이터를 저장할 수 있으며, 이를 이용하여 모델의 훈련 상태와 클러스터의 사용률을 확인할 수 있습니다.

### 주요 기능
- 자동으로 훈련 Metrics 및 모니터링
- 모델 훈련에 대한 가시성 제공
- 이상 상태 감지 및 분석

### Use case
예를 들어 BERT 모델 훈련 중 어텐션 매커니즘을 파악하고 싶을 때, 어텐션 스코어(attention score), 쿼리/키 벡터(query/key vectors)를 SageMaker Debugger로 캡처하여 어텐션 헤드(attention head)와 뉴런을 플롯할 수 있습니다.

아래 코드 예시를 참조해 주세요. `include_regex` 파라미터를 통해 정규 표현식으로 필요한 Tensor 만 저장한다는 점도 주목해 주세요.
```python
debugger_hook_config = DebuggerHookConfig(
    s3_output_path=s3_bucket_for_tensors,
    collection_configs=[
    CollectionConfig(
    name="all",
    parameters={
        "include_regex": 
          ".*multiheadattentioncell0_output_1|.*key_output|.*query_output",
        "train.save_steps": "0",
        "eval.save_interval": "10"}
    )]
)
```

## Amazon SageMaker Debugger Profiling

### Overview
Amazon SageMaker Debugger Profiling은 SageMaker 실행 코드에 아래와 같이 `ProfilerRule`과 `ProfilerConfig`를 추가하면, GPU, CPU, Memory 등에 대한 리소스 사용률을 모니터링할 수 있습니다. 

```python
from sagemaker.debugger import Rule
from sagemaker.debugger import rule_configs
from sagemaker.debugger import ProfilerRule

rules=[
    Rule.sagemaker(...),
    ProfilerRule.sagemaker(rule_configs.ProfilerReport()),
    ProfilerRule.sagemaker(rule_configs.CPUBottleneck()),
    ProfilerRule.sagemaker(rule_configs.GPUMemoryIncrease()),
    ProfilerRule.sagemaker(rule_configs.IOBottleneck()),
    ProfilerRule.sagemaker(rule_configs.LoadBalancing()),
    ProfilerRule.sagemaker(rule_configs.LowGPUUtilization()),
    ProfilerRule.sagemaker(rule_configs.OverallSystemUsage()),
    ...
]

from sagemaker.debugger import ProfilerConfig, FrameworkProfile

profiler_config = ProfilerConfig(
    system_monitor_interval_millis=500,
    framework_profile_params=FrameworkProfile(
	local_path="/opt/ml/output/profiler/", 
	start_step=5, 
	num_steps=10)
)

from sagemaker.tensorflow import TensorFlow

estimator = TensorFlow(
    ...
    rules=rules,
    debugger_hook_config=hook_config,
    profiler_config=profiler_config
)
```

### 주요 기능 
- 시스템 리소스(CPU, GPU, 네트워크 및 디스크 I/O)의 병목 현상 모니터링 및 리포팅
- 코드 수정 없이 리소스 프로파일링
- 리소스 최적화를 위한 권장 사항 제공

## Supported Frameworks and Algorithms
Debugger Profiling을 사용하려면 TensorFlow DL 컨테이너 2.3.1 이상, PyTorch 딥러닝 컨테이너 1.6.0 이상이 필요합니다. 

### Amazon SageMaker Debugger
- TensorFlow: AWS TensorFlow DL Container >= 1.15.4
- PyTorch: AWS PyTorch DL Container >= 1.5.0

### Amazon SageMaker Debugger Profiling
- TensorFlow: AWS TensorFlow DL Container >= 2.3.1
- PyTorch: AWS PyTorch DL Container >= 1.6.0
  
자세한 내용은 아래 웹 페이지를 참조하세요
- https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-supported-frameworks.html