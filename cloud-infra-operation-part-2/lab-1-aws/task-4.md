# Task 4 - 서버리스 서비스 백엔드

{% hint style="success" %}
실습에 사용되는 서비스&#x20;

DynamoDB : 어떤 규모에서도 10밀리초(10ms) 미만의 성능을 제공하는 완전관리형 NoSQL 키-값(Key-Value) 데이터베이스 서비스

IAM : AWS 리소스에 대한 인증(누구인지)과 권한 부여(무엇을 할 수 있는지)를 중앙에서 안전하게 관리하는 핵심 보안 서비스
{% endhint %}





1. 서비스 검색창에 DynamoDB를 검색하여 서비스 이동

<figure><img src="../../.gitbook/assets/image (504).png" alt=""><figcaption></figcaption></figure>





2. 테이블 생성 클릭

<figure><img src="../../.gitbook/assets/image (505).png" alt=""><figcaption></figcaption></figure>



3. 아래와 같이 설정후 테이블 생성 클릭

* 테이블 이름 : user\*\*-Rides
* 파티션 키 : user\*\*-RidesId
* 테이블 설정 : 기본 설정

<figure><img src="../../.gitbook/assets/image (506).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (507).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (508).png" alt="" width="244"><figcaption></figcaption></figure>





4. 테이블 생성이 완료되면 테이블 이름 클릭

<figure><img src="../../.gitbook/assets/image (509).png" alt=""><figcaption></figcaption></figure>



5. 하단의 Amazon 리소스 이름을 복사하여 메모장에 저장

<figure><img src="../../.gitbook/assets/image (510).png" alt=""><figcaption></figcaption></figure>







6. IAM 서비스로 이동

<figure><img src="../../.gitbook/assets/image (516).png" alt=""><figcaption></figcaption></figure>





7. 역할 생성 클릭

<figure><img src="../../.gitbook/assets/image (511).png" alt=""><figcaption></figcaption></figure>





8. AWS 서비스 > Lambda > Lambda 선택 후 다음 클릭

<figure><img src="../../.gitbook/assets/image (512).png" alt=""><figcaption></figcaption></figure>



9. 아래 정책명을 필터 검색한 뒤 선택하고 다음 클릭

{% code overflow="wrap" %}
```
AWSLambdaBasicExecutionRole
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (513).png" alt=""><figcaption></figcaption></figure>



10. 역할 이름에 user\*\*-WildRydesLambda 를 입력하고 역할 생성 클릭

<figure><img src="../../.gitbook/assets/image (514).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (515).png" alt="" width="239"><figcaption></figcaption></figure>





11. 역할에서 방금 만든 역할 클릭

<figure><img src="../../.gitbook/assets/image (517).png" alt=""><figcaption></figcaption></figure>





12. 권한 추가 클릭, 인라인 정책 생성 클릭

<figure><img src="../../.gitbook/assets/image (518).png" alt=""><figcaption></figcaption></figure>





13. 서비스 선택에서 dynamodb를 검색하여 선택

<figure><img src="../../.gitbook/assets/image (519).png" alt=""><figcaption></figcaption></figure>





14. 작업 검색란에 PutItem 을 검색하여 쓰기 란에 체크 한뒤, 하단 리소스 특정 선택, ARN 추가 클릭

<figure><img src="../../.gitbook/assets/image (520).png" alt=""><figcaption></figcaption></figure>





15. 리소스 ARN 에 이전에 메모장에 저장한 ARN 을 붙여넣기 (리전과 테이블이름은 자동완성)

<figure><img src="../../.gitbook/assets/image (521).png" alt=""><figcaption></figcaption></figure>





16. 다음을 클릭하고, 정책이름에 user\*\*-DynamoDBWriteAccess 를 입력한 뒤 정책 생성 클릭

<figure><img src="../../.gitbook/assets/image (522).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (523).png" alt=""><figcaption></figcaption></figure>





17. 람다 서비스로 이동

<figure><img src="../../.gitbook/assets/image (524).png" alt=""><figcaption></figcaption></figure>





18. 함수 생성 클릭&#x20;

<figure><img src="../../.gitbook/assets/image (525).png" alt=""><figcaption></figcaption></figure>

&#x20;

19. 아래와 같이 설정 한 뒤 함수 생성 클릭

* 새로 작성 선택
* 함수 이름 : user\*\*-RequestUnicorn
* 런타임 : Node.js 20.x
* 권한 하위 기본실행 역할 변경 확장
* 실행역할 : 다른 역할사용 선택 후 user\*\*-WildRydesLambda&#x20;

<figure><img src="../../.gitbook/assets/image (526).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (568).png" alt=""><figcaption></figcaption></figure>





20. 기존 index.mjs 의 소스코드를 지우고 아래 코드를 삽입

<pre class="language-json" data-overflow="wrap" data-line-numbers><code class="lang-json">const { randomBytes } = require('crypto');
const { DynamoDBClient } = require('@aws-sdk/client-dynamodb');
const { DynamoDBDocumentClient, PutCommand } = require('@aws-sdk/lib-dynamodb');

const client = new DynamoDBClient({});
const ddb = DynamoDBDocumentClient.from(client);

const fleet = [
    { Name: 'Bucephalus', Color: 'Golden', Gender: 'Male' },
    { Name: 'Shadowfax', Color: 'White', Gender: 'Male' },
    { Name: 'Rocinante', Color: 'Yellow', Gender: 'Female' },
];

exports.handler = (event, context, callback) => {
    if (!event.requestContext.authorizer) {
        errorResponse('Authorization not configured', context.awsRequestId, callback);
        return;
    }

    const rideId = toUrlString(randomBytes(16));
    const username = event.requestContext.authorizer.claims['cognito:username'];
    const requestBody = JSON.parse(event.body);
    const pickupLocation = requestBody.PickupLocation;
    const unicorn = findUnicorn(pickupLocation);

    recordRide(rideId, username, unicorn).then(() => {
        callback(null, {
            statusCode: 201,
            body: JSON.stringify({
                RideId: rideId,
                Unicorn: unicorn,
                UnicornName: unicorn.Name,
                Eta: '30 seconds',
                Rider: username,
            }),
            headers: { 'Access-Control-Allow-Origin': '*' },
        });
    }).catch((err) => {
        console.error(err);
        errorResponse(err.message, context.awsRequestId, callback);
    });
};

function findUnicorn(pickupLocation) {
    console.log('Finding unicorn for ', pickupLocation.Latitude, ', ', pickupLocation.Longitude);
    return fleet[Math.floor(Math.random() * fleet.length)];
}

function recordRide(rideId, username, unicorn) {
    return ddb.send(new PutCommand({
<strong>        TableName: 'user**-Rides',
</strong>        Item: {
<strong>            'user**-RidesId': rideId,
</strong>            User: username,
            Unicorn: unicorn,
            UnicornName: unicorn.Name,
            RequestTime: new Date().toISOString(),
        },
    }));
}

function toUrlString(buffer) {
    return buffer.toString('base64')
        .replace(/\+/g, '-')
        .replace(/\//g, '_')
        .replace(/=/g, '');
}

function errorResponse(errorMessage, awsRequestId, callback) {
    callback(null, {
        statusCode: 500,
        body: JSON.stringify({ Error: errorMessage, Reference: awsRequestId }),
        headers: { 'Access-Control-Allow-Origin': '*' },
    });
}
</code></pre>

<figure><img src="../../.gitbook/assets/image (528).png" alt=""><figcaption></figcaption></figure>

{% hint style="danger" %}
51,53 번 라인의 코드를 주의 하세요!
{% endhint %}



21. 코드 파일 명을 index.js 로 수정

<figure><img src="../../.gitbook/assets/image (197).png" alt=""><figcaption></figcaption></figure>





21. 테스트 탭으로 이동하여 이벤트이름에 user\*\*-TestRequestEvent 를 입력 한뒤 하단 이벤트 JSON 에 아래 코드로 수정&#x20;

<figure><img src="../../.gitbook/assets/image (529).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```json
{
    "path": "/ride",
    "httpMethod": "POST",
    "headers": {
        "Accept": "*/*",
        "Authorization": "eyJraWQiOiJLTzRVMWZs",
        "content-type": "application/json; charset=UTF-8"
    },
    "queryStringParameters": null,
    "pathParameters": null,
    "requestContext": {
        "authorizer": {
            "claims": {
                "cognito:username": "the_username"
            }
        }
    },
    "body": "{\"PickupLocation\":{\"Latitude\":47.6174755835663,\"Longitude\":-122.28837066650185}}"
}
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (530).png" alt=""><figcaption></figcaption></figure>



22. 테스트 클릭

<figure><img src="../../.gitbook/assets/image (531).png" alt=""><figcaption></figcaption></figure>





23. 세부정보를 확인

<figure><img src="../../.gitbook/assets/image (532).png" alt=""><figcaption></figcaption></figure>

아래와 같은 결과가 나왔는지 확인

{% code overflow="wrap" %}
```
{
  "statusCode": 200,
  "body": "\"Hello from Lambda!\""
}
```
{% endcode %}
