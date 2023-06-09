# 更新日志

## 2023-06-09 (Draft)

### 徐州仓库条码枪逻辑

1. 条码枪扫入钢板型号的二维码，通过 TCP 协议发送至 cranemagic-tcp-server。

2. cranemagic-servers-tcp 接收钢板型号后，生成发送给 MQTT Broker 的 Payload 如下：

    ```json
    {
        "type": "scanner_status",
        "steel_model": "xxxxxxxxx",
        "current_time": "%Y-%m-%d %H:%M:%S",
        "client_address": "IP:PORT",
    }
    ```

    其中 `type` 字段为枚举值，当前值 `scanner_status` 是标识当前有扫码枪扫入钢板型号的行为，另一值为 `scanner_inputs`，稍后详解其作用。

    将本 Payload 发送至 MQTT Broker 下的 2 个 Topic：`iot/scanner_inputs`（用以与 cranmegic-service-mqtt 服务交互）和 `web/scanner_status`（用以向前端报告当前有扫码枪扫入钢板型号的行为）。

3. 针对 MQTT 消息的处理

    a. 前端处理

    前端弹出 notification，报告当前有扫码枪扫入钢板型号的行为。

    b. cranemagic-service-mqtt 处理

    接收到条码枪扫入的钢板型号，验证钢板型号是否存在（目前的方式是查库位表中的`comment`）。

    查询对中平台 In 的材料状况：

    **如果对中平台 In 没有材料**，则：

    i. 向对中平台 In 新增相应钢板型号的材料。

    ii. 获取该钢板型号对应的仓储位信息，请求 cranemagic-service-webapi 服务的 `generateCraneTaskHandingWithoutAuth` 接口，得到生成路径。

    iii. 并通过 MQTT Topic `web/scanner_status` 前端报告当前可以执行入库操作，具体 Payload 如下：

    ```json
    {
        "type": "scanner_inputs",
        "steel_model": "xxxxxxxxx",
        "current_time": "%Y-%m-%d %H:%M:%S",
        "client_address": "IP:PORT",
        "action": "ready_for_inputs",
        "message": "",
        "material_id": "idxxxxxxxx",
        "area_id": "kwxx",
        "crane_control_payload": {
            // Crane_Control 的 Payload
        }
    }
    ```

    iv. 前端弹出 notification，点击可请求 cranemagic-service-webapi 服务的 `mqttPublishCraneControl` 接口快速执行入库操作。

    **如果对中平台 In 有且仅有 1 件材料**，则：

    i. 若钢板型号一致，则不做改动；若钢板型号不一致，删除原先 In 库位中的钢板，并向对中平台 In 新增相应钢板型号的材料。

    ii. 获取该钢板型号对应的仓储位信息，请求 cranemagic-service-webapi 服务的 `generateCraneTaskHandingWithoutAuth` 接口，得到生成路径。

    iii. 并通过 MQTT Topic `web/scanner_status` 前端报告当前可以执行入库操作，具体 Payload 如下：

    ```json
    {
        "type": "scanner_inputs",
        "steel_model": "xxxxxxxxx",
        "current_time": "%Y-%m-%d %H:%M:%S",
        "client_address": "IP:PORT",
        "action": "ready_for_inputs",
        "message": "the same steel_model" / "removed the older material",
        "material_id": "idxxxxxxxx",
        "area_id": "kwxx",
        "crane_control_payload": {
            // Crane_Control 的 Payload
        }
    }
    ```

    iv. 前端弹出 notification，点击可请求 cranemagic-service-webapi 服务的 `mqttPublishCraneControl` 接口快速执行入库操作。

    **如果对中平台 In 有大于 1 件材料**，则：

    i. 通过 MQTT Topic `web/scanner_status` 询问前端是否需要清除 In 库位的所有钢板，具体 Payload 如下：

    ```json
    {
        "type": "scanner_inputs",
        "steel_model": "xxxxxxxxx",
        "current_time": "%Y-%m-%d %H:%M:%S",
        "client_address": "IP:PORT",
        "action": "clear_in_area",
        "message": "x materials recorded in the In Area"
    }
    ```

    iv. 前端弹出 notification，...

