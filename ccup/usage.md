# 模型与用量

最后更新：2026-11-14

{% hint style="warning" %}
**所有链接都不用手机自带浏览器打开，都不用微信打开！！！**
{% endhint %}

加模型、关确认、查额度。前置：Claude Code 已经能用（见[安装](install.md)和[导入配置与续费](import.md)）。返回[总览](README.md)。

## 添加 fable-5

**视频教程链接：**

<https://v.douyin.com/WDwYe5cRVQ8/>

{% stepper %}
{% step %}

### 编辑 CC-Switch

打开 CC-Switch 编辑，并拉到下面输入：

`claude-fable-5`

![PixPin\_20260801\_161357.gif](../.gitbook/assets/imgsPixPin_20260801_161357.gif)

![PixPin\_20260624\_220812.png](../.gitbook/assets/imgsPixPin_20260624_220812.webp)

![PixPin\_20260702\_235348.png](../.gitbook/assets/imgsPixPin_20260702_235348.webp)
{% endstep %}

{% step %}

### 切换模型

回到 Claude code，可以通过 `/model` 命令切换模型。

![PixPin\_20260624\_221107.png](../.gitbook/assets/imgsPixPin_20260624_221107.webp)

![PixPin\_20260624\_221213.png](../.gitbook/assets/imgsPixPin_20260624_221213.webp)

![PixPin\_20260624\_221233.png](../.gitbook/assets/imgsPixPin_20260624_221233.webp)

![PixPin\_20260801\_164157.gif](../.gitbook/assets/imgsPixPin_20260801_164157.gif)
{% endstep %}
{% endstepper %}

## 一直点 Yes 太麻烦

`claude --dangerously-skip-permissions`

## 在 CC-Switch 里配置用量查询

配置好以后的效果如图所示：

<figure><img src="../.gitbook/assets/PixPin_20261011_134153.webp" alt=""><figcaption></figcaption></figure>

首先，我们可以点击 配置用量查询，然后，可以点击 启用用量查询，接着可以点击 通用模板 或 自定义

<div><figure><img src="../.gitbook/assets/PixPin_20261011_134349.webp" alt=""><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/PixPin_20261011_140319.webp" alt=""><figcaption></figcaption></figure></div>

接下来，只要将中转站的相关内容填写到下面的框体中即可，代码我放下面。

站点地址与激活码见[导入配置与续费](import.md)。

```javascript
({
  request: {
    url: "https://api.orbit1.lat/api/open/card/query",
    method: "POST",
    headers: {
      "Accept": "application/json",
      "Content-Type": "application/json"
    },
    body: JSON.stringify({
      card_key: "<你的激活码，需要手动替换>"
    })
  },

  extractor: function(response) {
    if (response.code !== 200 || !response.data) {
      return {
        isValid: false,
        invalidMessage: response.message || "查询失败"
      };
    }

    const data = response.data;
    const quota = data.quota || {};

    return {
      isValid: data.status === "active",
      invalidMessage: data.status_text || "卡密不可用",
      planName: data.product?.plan_name || "额度卡",
      remaining: Number(quota.remaining_usd),
      used: Number(quota.used_usd),
      total: Number(quota.total_usd),
      unit: "USD",
      extra: `请求次数：${quota.request_count ?? 0} 次`
    };
  }
})
```

<figure><img src="../.gitbook/assets/PixPin_20261011_140641 (1).webp" alt=""><figcaption></figcaption></figure>

填入后，点击测试脚本，若显示通过，则可以 保存配置。

旧版脚本已失效，留在[附录](appendix.md)备查。
