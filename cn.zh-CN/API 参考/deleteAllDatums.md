# deleteAllDatums {#concept_pt2_42v_42b .concept}

使用 deleteAllDatums 接口删除 ACM 配置。

## 请求类型 {#section_a2g_5cv_42b .section}

POST

## 请求 URL {#section_b2g_5cv_42b .section}

/diamond-server/datum.do

## 请求参数 {#section_f2y_wcv_42b .section}

|参数|类型|是否必需|描述|
|--|--|----|--|
|tenant|String|是|租户信息，对应 ACM 的命名空间 ID。|
|dataId|String|是|配置的 ID|
|group|String|是|配置的分组|

## Header 参数 {#section_m34_xcv_42b .section}

## 返回参数 {#section_n3m_ycv_42b .section}

|参数|类型|是否必需|描述|
|--|--|----|--|
|Spas-AccessKey|String|是|在 ACM 控制台上的命名空间详情对话框内可获取 AccessKey。|
|timeStamp|String|是|以毫秒为单位的请求时间。|
|Spas-Signature|String|是|使用 SecretKey 对“Tenant+Group+TimeStamp”签名（`SpasSigner.sign(tenant+ group+ timeStamp, secretKey)`），签名算法为 HmacSHA1。TimeStamp 签名的作用是防止重放攻击。该签名有效期为 60 秒。|

## 错误码 {#section_qxw_hpr_lgb .section}

|错误码|错误信息|描述|
|---|----|--|
|400|Bad Request|客户端请求中的语法错误|
|403|Forbidden|没有权限|
|404|Not Found|客户端错误，未找到。|
|500|Internal Server Error|服务器内部错误|

## 代码示例 {#section_jyd_1dv_42b .section}

-   请求示例（Shell）

    ``` {#codeblock_1f6_a3c_q1z .lanuage-shell}
    #!/bin/bash
    ## config param
    dataId="com.alibaba.nacos.example.properties2"
    group="DEFAULT_GROUP"
    namespace="04754ad1-4f67-4d67-b2bf-1f73a04a****"
    accessKey="8c5cbb849ae04682ad9f455a96aa****"
    secretKey="lwO5T7vfPJu27FclPa+/CyIG****"
    endpoint="acm.aliyun.com"
    ## config param end
    ## get serverIp from address server
    serverIp=`curl $endpoint:8080/diamond-server/diamond -s | awk '{a[NR]=$0}END{srand();i=int(rand()*NR+1);print a[i]}'`
    ## config sign
    timestamp=`echo $[$(date +%s%N)/1000000]`
    signStr=$group+$timestamp
    signContent=`echo -n $signStr | openssl dgst -hmac $secretKey -sha1 -binary | base64`
    ## request to delete a config
    curl -X POST -H "Spas-AccessKey:"$accessKey -H "timeStamp:"$timestamp -H "Spas-Signature:"$signContent "http://"$serverIp":8080/diamond-server/datum.do?method=deleteAllDatums" -d "dataId="$dataId"&group="$group -v
    ```

-   返回示例

    ``` {#codeblock_45b_v8r_o7l}
    True
    ```


## 更多信息 {#section_obg_5mx_2y6 .section}

[API 概览](intl.zh-CN/API 参考/API 概览.md#)

