# 创建 Managed Kubernetes集群 {#reference_af2_kc1_dfb .reference}

创建一个新的 Managed Kubernetes 集群实例，并创建指定数量的节点。

## 请求信息 {#section_dt4_pl2_xdb .section}

**请求行 RequestLine**

``` {#codeblock_auw_0g7_1q6}
POST /clusters HTTP/1.1 
```

**特有请求头 RequestHead**

无，请参考[公共请求头部](intl.zh-CN/开发指南/集群 API 调用方式/公共参数.md#section_mr5_lf1_wdb)。

**请求体 RequestBody**

``` {#codeblock_6by_0xo_29f}
{
"disable_rollback": "失败是否回滚",
"name": "集群名称",
"timeout_mins": 集群创建超时时间,
"cluster_type": "集群类型，ManagedKubernetes",
"region_id": "地域",
"vpcid": "VPC ID",
"vswitch_ids": "一台或多台虚拟交换机 ID，N 的取值范围为 [1, 5]",
"container_cidr": "容器POD CIDR",
"service_cidr": "服务CIDR",
"cloud_monitor_flags":"是否安装云监控插件",
"login_password": "节点SSH登陆密码，和key_pair二选一",
"key_pair":"keypair名称，和login_password 二选一",
"worker_instance_charge_type":"Worker节点付费类型PrePaid|PostPaid",
"worker_period_unit":"包年包月单位，Month,Year，只有在PrePaid下生效",
"worker_period":"包年包月时长，只有在PrePaid下生效",
"worker_auto_renew":"Worker节点自动续费true|false",
"worker_auto_renew_period":"Worker节点续费周期",
"worker_instance_types": "Worker实例规格多实例规格参数. ",
"worker_system_disk_category": "Worker系统盘类型",
"worker_system_disk_size": "Worker节点系统盘大小",
"worker_data_disk":"是否挂载数据盘 true|false",
"worker_data_disk_category":"数据盘类型",
"worker_data_disk_size":"数据盘大小",
"num_of_nodes": "Worker节点数",
"snat_entry": 是否配置SNATEntry,
"endpoint_public_access":"是否公网暴露集群endpoint",
"proxy_mode": "网络模式, 可选值iptables|ipvs",
"addons": "选装addon, 数组格式对象", 
"tags": "给集群打tag标签, 数组格式对象",
}
```

**请求体解释**

|名称|类型|必须|描述|
|--|--|--|--|
|cluster\_type|string|是|集群类型|
|key\_pair|string|是|keypair名称。与login\_password二选一|
|login\_password|string|是|SSH登录密码。密码规则为8 - 30 个字符，且同时包含三项（大、小写字母，数字和特殊符号）。和key\_pair 二选一|
|name|string|是|集群名称，集群名称可以使用大小写英文字母、中文、数字、中划线|
|num\_of\_nodes|int|是|Worker节点数。范围是\[0,300\]|
|region\_id|string|是|集群所在地域ID|
|snat\_entry|bool|是|是否为网络配置SNAT。如果是自动创建VPC必须设置为true。如果使用已有VPC则根据是否具备出网能力来设置|
|vswitch\_ids|list|是|交换机ID。List长度范围为 \[1, 3\]|
|worker\_system\_disk\_category|string|是|Worker节点系统盘类型|
|worker\_system\_disk\_size|int|是|Worker节点系统盘大小|
|addons|list|否|Kubernetes集群的addon插件的组合。 -   addons的参数：
    -   name：必填，addon插件的名称
    -   version：可选，取值为空时默认取最新版本
    -   config：可选，取值为空时表示无需配置
-   网络插件：包含Flannel和Terway网络插件，二选一
-   日志服务：可选，如果不开启日志服务时，将无法使用集群审计功能

 |
|container\_cidr|string|否|容器网段，不能和VPC网段冲突。当选择系统自动创建VPC时，默认使用172.16.0.0/16网段|
|cloud\_monitor\_flags|bool|否|是否安装云监控插件|
|disable\_rollback|bool|否|失败是否回滚： -   true：表示失败不回滚
-   false：表示失败回滚

 如果选择失败回滚，则会释放创建过程中所生产的资源，不推荐使用false|
|proxy\_mode|string|否|kube-proxy代理模式，支持iptables和IPVS两种模式。 默认为iptables。|
|endpoint\_public\_access|bool|否|是否开启公网API Server： -   true：默认为True，表示开放公网API Server
-   false：若设置为false， 则不会创建公网的API Server，仅创建私网的API Server

 |
|service\_cidr|string|否|服务网段，不能和VPC网段以及容器网段冲突。当选择系统自动创建VPC时，默认使用172.19.0.0/20网段|
|tags|list|否|给集群打tag标签： -   key：标签名称
-   value：标签值

 |
|timeout\_mins|int|否|集群资源栈创建超时时间，以分钟为单位，默认值 60分钟|
|vpcid|string|否|VPC ID，可空。如果不设置，系统会自动创建VPC，系统创建的VPC网段为192.168.0.0/16。 VpcId 和 vswitchid 只能同时为空或者同时都设置相应的值|
|worker\_auto\_renew|bool|否|是否开启Worker节点自动续费，可选值为： -   true：自动续费
-   false：不自动续费

 |
|worker\_auto\_renew\_period|int|否|自动续费周期，当worker\_instance\_charge\_type取值为PrePaid时才生效且为必选值： -   `PeriodUnit=Week`时，取值：\{“1”, “2”, “3”\}，
-   `PeriodUnit=Month`时，取值\{“1”, “2”, “3”, “6”, “12”\}

 |
|worker\_data\_disk|string|否|是否挂载数据盘，可选择： -   true：表示worker节点挂载数据盘
-   false：表示worker节点不挂载数据盘

 |
|worker\_data\_disk\_category|int|否|数据盘类型|
|worker\_data\_disk\_size|string|否|数据盘大小|
|worker\_instance\_charge\_type|string|否|Worker节点付费类型，可选值为： -   PrePaid：预付费
-   PostPaid：按量付费

 |
|worker\_period|int|否|包年包月时长，当worker\_instance\_charge\_type取值为PrePaid时才生效且为必选值，取值范围： -   `PeriodUnit=Week`时，Period取值：\{“1”, “2”, “3”, “4”\}，
-   `PeriodUnit=Month`时，Period取值：\{ “1”, “2”, “3”, “4”, “5”, “6”, “7”, “8”, “9”, “12”, “24”, “36”,”48”,”60”\}

 |
|worker\_period\_unit|string|否|当指定为PrePaid的时候需要指定周期。可选择为： -   Week：以周为计时单位
-   Month：以月为计时单位

 |

## 返回信息 {#section_ljv_tm2_xdb .section}

**返回行 ResponseLine**

``` {#codeblock_d6k_kji_qjz}
HTTP/1.1 202 Accepted
```

**特有返回头 ResponseHead**

无，请参考[公共返回头部](intl.zh-CN/开发指南/集群 API 调用方式/公共参数.md#section_zr5_lf1_wdb)。

**返回体 ResponseBody**

``` {#codeblock_v9b_2dx_vcr}
{
"cluster_id":"string",
"request_id":"string",
"task_id":"string"
}
```

## 示例 {#section_hcw_wm2_xdb .section}

**请求示例**

``` {#codeblock_6n5_chh_yez}
POST /clusters HTTP/1.1
<公共请求头>
{
"name":"test",
"cluster_type":"my-test-Kubernetes-cluster",
"disable_rollback":true,
"timeout_mins":60,
"kubernetes_version":"1.12.6-aliyun.1",
"region_id":"cn-beijing",
"snat_entry":true,
"cloud_monitor_flags":false,
"endpoint_public_access":false,
"node_cidr_mask":"25",
"proxy_mode":"ipvs",
"tags":[],
"addons":[{"name":"flannel"}, {"name":"nginx-ingress-controller"}],
"worker_instance_types":["ecs.hfc5.xlarge"],
"num_of_nodes":3,
"worker_system_disk_category":"cloud_efficiency",
"worker_system_disk_size":120,
"worker_instance_charge_type":"PostPaid",
"vpcid":"vpc-2zegvl5etah5requ09nec",
"container_cidr":"172.20.0.0/16",
"service_cidr":"172.21.0.0/20",
"vswitch_ids":["vsw-2ze48rkq464rsdts1****"],
"login_password":"test@19****"
}
```

**返回示例**

``` {#codeblock_f1y_4h5_mn4}
HTTP/1.1 202 Accepted
<公共响应头>
{
    "cluster_id": "cb95aa626a47740afbf6aa099b65****",
    "request_id": "687C5BAA-D103-4993-884B-C35E4314A1E1",
    "task_id": "T-5a54309c80282e39ea00002f"
}
```

