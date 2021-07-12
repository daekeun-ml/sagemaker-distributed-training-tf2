## Training MaskRCNN(COCO-2017 dataset) using SageMaker DataParallel

Source: https://github.com/mullue/tf2-sm-ddp

## Overview
본 핸즈온은 Object Detection 분야에서 많이 활용되는 COCO-2017 dataset을 Amazon FSx for Luster 파일 시스템으로 가져와서 SageMaker Data Parallelism 라이브러리를 사용하여 분산 훈련을 수행합니다.

### Why Amazon FSx for Lustre?
일반적으로 클라이언트에서 SageMaker 훈련 작업을 호출 시 1) 훈련 리소스 프로비저닝, 2) S3와 직접 통신하여 훈련 데이터, 훈련 컨테이너 및 훈련 코드 다운로드, 3) 훈련 수행, 4) 훈련이 완료된 모델 아티팩트(model.tar.gz) S3로 복사하는 과정을 거칩니다. 이 때 훈련 데이터셋의 크기가 수십 기가~테라바이트 단위일 때 2)에서 소요되는 시간은 기하급수적으로 늘어나게 됩니다.

Amazon FSx for Lustre는 기본적으로 S3와 통합되는 고성능 POSIX(포직스,Portable Operating System Interface) 호환 완전 관리형 파일 시스템입니다. 데이터셋 크기와 상관 없이 약 10여분 만에 S3 버킷의 훈련 데이터를 FSx 파일 시스템으로 마운트한 이후에는 여러 인스턴스의 데이터에 동시에 액세스하고 S3 object를 캐시합니다. 따라서, SageMaker 훈련 작업을 시작할 때마다 훈련 데이터 다운로드에 걸리는 시간을 없애고 우수한 데이터 읽기 처리량(throughput)을 제공하며 이는 SageMaker에서 훈련 시간 단축으로 이어집니다.

### Hands-on Lab 
- `TF_DP_CFN.yaml`: 본 핸즈온랩에 필요한 AWS 리소스를 CloudFormation으로 생성합니다. CloudFormation 스택은 아래 리소스를 자동으로 생성합니다.
  - EC2 및 SageMaker 인스턴스에 private 서브넷(subnet) + 보안 그룹(security group)이 있는 VPC
  - AWS 리소스에 액세스하는 데 필요한 IAM role
  - Jupyter 노트북에서 모델을 정의하는 SageMaker 노트북 인스턴스 (`ml.t2.xlarge`)
  - SageMaker에 필요한 S3 버킷
  - 분산 훈련을 위한 FSx for Lustre 파일 시스템
- `tf2-mrcnn-dataparallel.ipynb`: 핸즈온 실습에 필요한 주피터 노트북

## CloudFormation 없이 밑바닥부터 FSx를 구성 시 주의 사항

- SageMaker 노트북 인스턴스를 시작할 때 FSx에서 사용하는 것과 동일한 **서브넷**, **vpc** 및 **보안 그룹**을 사용해야 합니다.
- SageMaker가 훈련 작업(training job)에서 FSx 파일 시스템에 액세스하려면 보안 그룹 에서 적절한 **인바운드/아웃바운드 규칙**을 설정해야 합니다. 참고로 FSx를 허용하기 위한 포트 범위는 988, 1021-1023 입니다. (참조: https://docs.aws.amazon.com/fsx/latest/LustreGuide/limit-access-security-groups.html)
- SageMaker IAM Role에서 `AmazonFSxFullAccess` Policy를 attach합니다.

## References
### AWS Documentations
- Linking your file system to an S3 bucket: https://docs.aws.amazon.com/fsx/latest/LustreGuide/create-fs-linked-data-repo.html
- Speed up training on Amazon SageMaker using Amazon FSx for Lustre and Amazon EFS file systems: https://aws.amazon.com/blogs/machine-learning/speed-up-training-on-amazon-sagemaker-using-amazon-efs-or-amazon-fsx-for-lustre-file-systems/

### Original Implementation
- https://github.com/mullue/tf2-sm-ddp