# AWS Identity & Access Management(AWS IAM)

### IAM: Users & Groups

- IAM: Identity and Access Management
- Root 계정은 기본값으로 존재하고, 사용되어선 안되고 공유되어선 안된다.
- 조직 내에 있는 사용자들은 그룹핑 될 수 있다.
- 그룹은 오직 사용자들을 포함시킬 수 있고, 다른 그룹은 포함시킬 수 없다.
- 사용자는 여러 개의 그룹에 속할 수 있다.

### IAM: Permissions

- 사용자들 또는 그룹은 '**정책**'이라고 불리는 JSON 문서에 할당될 수 있다.
- 이 정책들은 사용자에 대한 허가증이다.
- AWS에서 사용자는 최소권한원칙을 적용할 수 있다. -> 사용자가 필요로하는 허용범위에서 더 많은 권한을 부여하지 말아야한다.

### IAM: Policies Structure

**전체적인 구성**

- Version: 정책 언어의 버전, 항상 "2012-10-17"을 포함한다.
- Id: 정책의 고유 ID를 의미한다.(선택)
- Statement: 하나 이상의 개별적인 statement를 이야기한다.(필수)

**Statement 구성**

- Sid: statement의 ID(선택)
- Effect: 이 statement를 허용하는지 거부하는지 여부
- Principal: 이 정책이 적용될 **계정,사용자,역할**을 의미한다.
- Action: 허용 또는 거부할 **작업들**을 의미한다.
- Resource: 이 작업들이 **적용될 리소스들**을 의미한다.
- Condition: 이 정책이 적용될 조건을 의미한다.

### IAM Roles for Services

- 몇몇 AWS 서비스들은 스스로 동작하기 위해서 추가적인 조치가 필요하다.
- 이를 위해서 IAM 역할로서 AWS 서비스들 조작할 수 있는 권한을 부여해야 한다.
- 이를 IAM Role이라고 한다.

### 요약

- Users: 실제 사용자를 의미한다. AWS Console 에서 로그인할 수 있는 비밀번호가 주어진다.
- Groups: 사용자만이 속할 수 있다.
- Polices: 사용자 또는 그룹을 위한 허가증을 의미하는 JSON 문서이다.
- Roles: AWS 서비스들이 스스로 다른 서비스를 제어할 때 필요한 것이다.
- Security: MFA + Password Policy
- AWS CLI: 커맨드로 AWS 서비스를 제어할 수 있는 서비스
- AWS SDK: 프로그래밍 언어로 AWS 서비스들을 제어할 수 있는 서비스
- Access Keys: CLI 또는 SDK를 사용하여 AWS에 액세스할 수 있는 키