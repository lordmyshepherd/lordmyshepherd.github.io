---
title: "Django에서 AWS S3에 파일  업로드 엔드포인트 만들기"
date: "2019-11-14T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/AWS_S3"
category: "Django"
tags:
description: "#DoesNotExist vs ObjectsDoesNotExist"
socialImage: ""
---
*The Lord is my shepherd, I lack nothing. PSLAM 23:1*

### Django에서 AWS S3에 파일 업로드 과정
AWS IAM setup--> AWS bucket setup--> 엔드포인트 using Django boto3

### AWS IAM (Identity and Access Management)  
+ 사용자와 정책  
   IAM 사용자는 서비스의 자격 증명입니다.  IAM 사용자를 생성할 경우, 권한을 부여하지 않는 한 사용자는 계정 내에서 어떠한 것으로도 액세스할 수 없습니다. 사용자 또는 사용자가 속한 그룹에 연결된 정책인 자격 증명 기반 정책을 생성하여 사용자에게 권한을 부여합니다. 다음 예는 사용자가 us-east-2 리전 내의 123456789012 계정에서 Books 테이블의 모든 Amazon DynamoDB 작업(dynamodb:*)을 수행할 수 있도록 허용하는 JSON 정책을 보여 줍니다.
    ```python
    {
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Allow",
        "Action": "dynamodb:*",
        "Resource": "arn:aws:dynamodb:us-east-2:123456789012:table/Books"
    }
    }
    ```
    이 정책을 IAM 사용자에게 연결한 후에는 해당 사용자가 이러한 DynamoDB 권한만 부여받습니다. 대부분의 사용자는 해당 사용자의 권한을 함께 나타내는 여러 정책을 부여받습니다.
+ SETTING  
    **<정책>**  
    [STEP 1] IAM 정책 페이지에서 새로운 정책 생성  
    [STEP 2] 새로운 permission policy를 추가하는 페이지에서 JSON 탭으로 이동해서 다음의 JSON을 입력  
    ```python
        {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Sid": "VisualEditor0",
                    "Effect": "Allow",
                    "Action": [
                        "s3:PutObject",
                        "s3:GetObjectAcl",
                        "s3:GetObject",
                        "s3:ListBucket",
                        "s3:DeleteObject",
                        "s3:PutObjectAcl"
                    ],
                    "Resource": [
                        "arn:aws:s3:::example-bucket-name/*",
                        "arn:aws:s3:::example-bucket-name"
                    ]
                }
            ]
        }
    ```
    ***example-bucket-name*** 부분은 나중에 S3 버켓을 생성했을때 해당 S3 버켓이름으로 바꾸어 주도록 한다.  
    [STEP 3] 정책 이름과 설명 추가  
    [STEP 4] 정책 생성 확인

    ![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/Django/1.png)

    **<사용자>**  
    [STEP 1] 사용자 생성  
    [STEP 2] 사용자 추가 페이지로 이동한다.  
    [STEP 3] Programmatic Access 항목을 체크한다.  
    [STEP 4] 만들어 놓은 정책을 선택하고 다음으로 넘어간다.  
    [STEP 5] PASS  
    [STEP 6] PASS  
    [STEP 7] 새로운 유저 생성이 완료되면 "Download. csv" 버튼을 클릭해서 유저의 "Access Key ID"와 "Secret Access Key"를 다운로드 받도록 한다. 지금 다운 받지 않으면 다시 다운 받거나 찾을 수 없음으로 꼭 다운 받도록 하자.


    ![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/Django/s3_user.png)

### AWS S3 bucket 

+ bucket?
버킷은 Amazon S3에 저장된 객체에 대한 컨테이너입니다. 모든 객체는 어떤 버킷에 포함됩니다. 예를 들어, photos/puppy.jpg로 명명된 객체는 johnsmith 버킷에 저장되며, URL http://johnsmith.s3.amazonaws.com/photos/puppy.jpg를 사용하여 주소를 지정할 수 있습니다.

+ AWS S3 bucket setting  
[STEP 1 ~ 2] AWS S3 페이지로 가서 "Create bucket" 버튼을 클릭한 후 버켓 이름과 region을 선택  
[STEP 3] "Block all public access"를 uncheck  
[STEP 4 ~ 5] 버켓 생성이 완료되면 해당 버켓을 클릭해서 버켓의 설정 페이지로 이동  
[STEP 6] 상단의 "Permission(권한)" 탭에서 "Bucket Policy(버킷 정책)" 탭을 눌른 후 다음의 JSON을 입력  
```python
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "Stmt1507637373230",
                "Effect": "Allow",
                "Principal": {
                    "AWS": "{user_arn}"
                },
                "Action": [
                    "s3:GetObject",
                    "s3:PutObject",
                    "s3:DeleteObject"
                ],
                "Resource": "arn:aws:s3:::{bucket_name}/*"
            },
            {
                "Sid": "Stmt1507637391106",
                "Effect": "Allow",
                "Principal": "*",
                "Action": "s3:GetObject",
                "Resource": "arn:aws:s3:::{bucket_name}/*"
            }
        ]
    }    
```
***{user_arn}*** 은 IAM user의 ARN 이고, ***{bucket_name}***은 각자 버켓의 이름이다.  
I AM 유저의 ARN은 IAM 페이지에서 users 페이지에 해당 유저를 클릭하면 나오는 페이지에서 확인 할 수 있다.  
그리고 save 버튼을 눌르자. Save 버튼을 눌르면 Public access를 주지 말라는 메세지의 warning이 뜬다. 지금은 무시해도 좋다.
```python
원칙적으로는 S3는 public access를 주지 않는것이 보안상 좋다. S3는 private하게 막고 S3 앞에 CloudFront를 붙혀서 CDN 기능을 구현하는것이 정석이긴 하다.
```
[STEP 7] "CORS configuration" 버튼을 눌러서 아래와 같이 CORS 설정 후 save  
```python
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
<CORSRule>
    <AllowedOrigin>*</AllowedOrigin>
    <AllowedMethod>GET</AllowedMethod>
    <MaxAgeSeconds>3000</MaxAgeSeconds>
    <ExposeHeader>ETag</ExposeHeader>
    <AllowedHeader>*</AllowedHeader>
</CORSRule>
</CORSConfiguration>
```
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/Django/s3_bucket.png)

### Django에서 boto3를 사용하여 S3와 연결 및 파일 업로드
[STEP 1] python용 AWS 라이브러리인 boto3 설치
```python
pip install boto3
```
[STEP 2] boto3 라이브러리를 사용하여 S3에 파일 업로드 엔드포인트 코드
```python
class FileView(View):

    s3_client = boto3.client(
        's3',
        aws_access_key_id="AKIASTLUR2MMQHRRSS6M",
        aws_secret_access_key="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"
    )

    def post(self, request):
        file = request.FILES['filename']

        self.s3_client.upload_fileobj(
            file, 
            "weandb",
            file.name,
            ExtraArgs={
                "ContentType": file.content_type
            }
        ) 

        return HttpResponse(status= 200)
```
   + ***aws_access_key_id*** 와 ***aws_secret_access_key*** 값은 IAM user 생성후 다운 받았던 csv 파일에 나와있다.
   + 'filename' : client에서 file을 FILES에 key : value 형식으로 보내므로 보내는 파일을 꺼낼 수 있는 key 값이다.
   + ***upload_fileobj 함수***의 두번째 인자는 파일을 업로드 하기 희망하는 버켓의 이름이다.
   + client로부터 전달받은 파일을 ***request.FILES***를 통해 AWS S3로 전달된다.

### Database에 파일이 저장된 S3의 url 저장
```python
host_image_url = "https://s3.ap-northeast-2.amazonaws.com/weandb/"+file_name
```
   + ap-northeast-2 : 한국 
   + weandb : 해당 파일이 저장되는 버킷
   
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/Django/file_url.png)