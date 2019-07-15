---
title: PCI DSS简介
date: 2019-07-01 09:24:37
tags: [pci]
---

[PCI DSS][] (支付卡行业数据安全标准) 旨在促进并增强持卡人的数据安全，便于统一的数据安全措施在全球范围内的广泛应用。
[PCI DSS][] 为意在保护帐户数据的技术和操作要求提供了一个基准。
[PCI DSS][] 适用于参与支付卡处理的所有实体 — 包括商户、处理商、收单机构、发卡机构和服务提供商。
[PCI DSS][] 还适用于存储、处理或传输持卡人数据 (CHD) 和/或敏感验证数据 (SAD) 的所有其他实体。

<table style="width: 720px; margin: auto;">
    <tr>
        <td><b>建立并维护安全的网络和系统</b></td>
        <td>1. 安装并维护防火墙配置以保护持卡人数据<br/>2. 不要使用供应商提供的默认系统密码和其他安全参数</td>
    </tr>
    <tr>
        <td><b>保护持卡人数据</b></td>
        <td>3. 保护存储的持卡人数据<br/>4. 加密持卡人数据在开放式公共网络中的传输</td>
    </tr>
    <tr>
        <td><b>维护漏洞管理计划</b></td>
        <td>5. 为所有系统提供恶意软件防护并定期更新杀毒软件或程序<br/>6. 开发并维护安全的系统和应用程序</td>
    </tr>
    <tr>
        <td><b>实施强效访问控制措施</b></td>
        <td>7. 按业务知情需要限制对持卡人数据的访问<br/>8. 识别并验证对系统组件的访问<br/>9. 限制对持卡人数据的物理访问</td>
    </tr>
    <tr>
        <td><b>定期监控并测试网络</b></td>
        <td>10. 跟踪并监控对网络资源和持卡人数据的所有访问<br/>11. 定期测试安全系统和流程</td>
    </tr>
    <tr>
        <td><b>维护信息安全政策</b></td>
        <td>12. 维护针对所有工作人员的信息安全政策</td>
    </tr>
</table>

[PCI DSS]: https://zh.pcisecuritystandards.org/
[PCI_DSS_v3-2_zh-CN]: https://zh.pcisecuritystandards.org/_onelink_/pcisecurity/en2zhcn/minisite/en/docs/PCI_DSS_v3-2_zh-CN.pdf
