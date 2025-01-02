对于这种未来可能会变动的多返回值函数来说，如果一开始就使用 NamedTuple 类型对返回结果进行建模，兼容性会好不少。

示例：
```
from typing import NamedTuple

class Address(NamedTuple):
    """地址信息结果"""
    country: str
    province: str
    city: str

def latlon_to_address(lat, lon):
    return Address(
        country=country,
        province=province,
        city=city,
    )

addr = latlon_to_address(lat, lon)
# 通过属性名来使用 addr
# addr.country / addr.province / addr.city
```