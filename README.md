In this repo I am trying to explain that when the following is installed and enabled the project doesn't work when deployed to aws lambda

1. [serverless-layers](https://github.com/agutoli/serverless-layers)
2. [serverless-plugin-datadog](https://github.com/DataDog/serverless-plugin-datadog)
3. [@nestjs/swagger](https://github.com/nestjs/swagger)

if you disable any of them everything works fine (except the disabled protion ofc)

To test this out

1. Add `DATADOG_API_KEY` env var with your datadog env vars
2. Add AWS key id and secret to your env vars `AWS_ACCESS_KEY_ID` & `AWS_SECRET_ACCESS_KEY`
3. run `npm i`
4. run `npx serverless deploy --verbose`
5. visit the url from the console output

It should display swagger but it doesn't instead `{"message": "Internal server error"}`

see bottom for full output

if you append `/v1` to the url it should show `Hello World!` but it doesn't

i.e. https://qwerty.execute-api.us-east-2.amazonaws.com/dev/ should show swagger
i.e. https://qwerty.execute-api.us-east-2.amazonaws.com/dev/v1 should show `Hello World!`

The three above combined break the lambda function

P.S. if you switch to any of the following branches the project works as expected

1. `disable-serverless-layers-works` [preview changes](https://github.com/Shereef/nestjs-swagger-serverless-layers-datadog-issue/compare/main...disable-serverless-layers-works)
2. `disable-datadog-works` [preview changes](https://github.com/Shereef/nestjs-swagger-serverless-layers-datadog-issue/compare/main...disable-datadog-works)
3. `disable-swagger-works` [preview changes](https://github.com/Shereef/nestjs-swagger-serverless-layers-datadog-issue/compare/main...disable-swagger-works)

```
Test Event Name
root

Response
{
  "errorType": "TypeError",
  "errorMessage": "pathToRegexp.parse is not a function or its return value is not iterable",
  "trace": [
    "TypeError: pathToRegexp.parse is not a function or its return value is not iterable",
    "    at SwaggerExplorer.validateRoutePath (/opt/nodejs/node_modules/@nestjs/swagger/dist/swagger-explorer.js:156:41)",
    "    at /opt/nodejs/node_modules/@nestjs/swagger/dist/swagger-explorer.js:136:35",
    "    at Array.map (<anonymous>)",
    "    at SwaggerExplorer.exploreRoutePathAndMethod (/opt/nodejs/node_modules/@nestjs/swagger/dist/swagger-explorer.js:135:30)",
    "    at /opt/nodejs/node_modules/@nestjs/swagger/dist/swagger-explorer.js:72:45",
    "    at Array.reduce (<anonymous>)",
    "    at /opt/nodejs/node_modules/@nestjs/swagger/dist/swagger-explorer.js:71:104",
    "    at /opt/nodejs/node_modules/lodash/lodash.js:13469:38",
    "    at /opt/nodejs/node_modules/lodash/lodash.js:4967:15",
    "    at baseForOwn (/opt/nodejs/node_modules/lodash/lodash.js:3032:24)"
  ]
}

Function Logs
START RequestId: d721794d-11ac-1234-a0ef-acc07be0df1e Version: $LATEST
[32m[Nest] 15  - [39m12/06/2022, 4:10:59 AM [32m    LOG[39m [38;5;3m[NestFactory] [39m[32mStarting Nest application...[39m[38;5;3m +43001ms[39m
[32m[Nest] 15  - [39m12/06/2022, 4:10:59 AM [32m    LOG[39m [38;5;3m[InstanceLoader] [39m[32mAppModule dependencies initialized[39m[38;5;3m +23ms[39m
2022-12-06T04:11:00.034Z	d721794d-11ac-1234-a0ef-acc07be0df1e	ERROR	[dd.trace_id=2179485768246681000 dd.span_id=4424661968803014230] Invoke Error 	{"errorType":"TypeError","errorMessage":"pathToRegexp.parse is not a function or its return value is not iterable","stack":["TypeError: pathToRegexp.parse is not a function or its return value is not iterable","    at SwaggerExplorer.validateRoutePath (/opt/nodejs/node_modules/@nestjs/swagger/dist/swagger-explorer.js:156:41)","    at /opt/nodejs/node_modules/@nestjs/swagger/dist/swagger-explorer.js:136:35","    at Array.map (<anonymous>)","    at SwaggerExplorer.exploreRoutePathAndMethod (/opt/nodejs/node_modules/@nestjs/swagger/dist/swagger-explorer.js:135:30)","    at /opt/nodejs/node_modules/@nestjs/swagger/dist/swagger-explorer.js:72:45","    at Array.reduce (<anonymous>)","    at /opt/nodejs/node_modules/@nestjs/swagger/dist/swagger-explorer.js:71:104","    at /opt/nodejs/node_modules/lodash/lodash.js:13469:38","    at /opt/nodejs/node_modules/lodash/lodash.js:4967:15","    at baseForOwn (/opt/nodejs/node_modules/lodash/lodash.js:3032:24)"]}
2022-12-06 04:11:00 UTC | DD_EXTENSION | ERROR | LogMessage.UnmarshalJSON: can't read the spans object
END RequestId: d721794d-11ac-1234-a0ef-acc07be0df1e
REPORT RequestId: d721794d-11ac-1234-a0ef-acc07be0df1e	Duration: 276.08 ms	Billed Duration: 277 ms	Memory Size: 1024 MB	Max Memory Used: 175 MB

Request ID
d721794d-11ac-1234-a0ef-acc07be0df1e
```
