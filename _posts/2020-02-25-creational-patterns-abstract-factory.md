---
layout: post
title: Creational Patterns - Abstract Factory
---

Nếu Factory Method cung cấp một phương thức để khởi tạo các đối tượng từ một lớp cha. Thì Abstract Factory chẳng qua là "số nhiều" của Factory Method, nó cung cấp nhiều phương thức để khởi tạo nhiều đối tượng (được xác định là có liên quan đến một hệ thống nào đó) từ một lớp cha. Tất nhiên các đối tượng này cũng được định nghĩa lại ở các lớp con. 

Cũng chính vì điều này mà lớp cha sẽ không còn các phương thức xử lý vì khi đó nhiều đối tượng có thể sẽ có những phương thức giống nhau. Nói cách khác, lớp cha lúc này sẽ tiêu biến từ abstract class để trở thành 1 interface. Okie chúng ta sẽ phát triển tiếp bài toán của Factory Method để hiểu rõ vấn đề...

Quay lại bài toán Logistics
-------------

Lần trước sau khi sửa lại code và chúng ta có thể tích hợp các đơn vị vận chuyển đường biển vào hệ thống. Rất tốt doanh thu đã tăng kha khá rồi. Tuy nhiên bây giờ các khách hàng của chúng ta đã lớn mạnh và họ lại vừa có thể vận chuyển đường biển lại vừa có thể vận chuyển đường bộ. Một số đối tác sang mảng mới họ làm chưa tốt và chúng ta nhận được rất nhiều phản hồi từ khách hàng. Vậy chúng ta cần quản lý các đơn vị vận chuyển này như nào?

Giải pháp
------------

Đầu tiên ta sửa lại lớp Logistic, biến nó thành interface với 2 phương thức là createRoadTransport() và createSeaTransport(), như vậy từ giờ một đối tác vận chuyển sẽ có thể vận chuyển đường bộ và đường biển.

```php
<?php

namespace Modules\Services\Logistic;

interface Logistic
{
   public function createRoadTransport(): RoadTransport;
   public function createSeaTransport(): SeaTransport;
}
```

Tiếp đến chúng ta sẽ tạo các đối tác vận chuyển.

```php
<?php
namespace Modules\Services\Logistic;

class ShippingPartner1 implements Logistic
{
    public function createRoadTransport(): RoadTransport;
    {
        return new RoadTransport1;
    }

    public function createSeaTransport(): SeaTransport;
    {
        return new SeaTransport1;
    }
}

class ShippingPartner2 implements Logistic
{
    public function createRoadTransport(): RoadTransport;
    {
        return new RoadTransport2;
    }

    public function createSeaTransport(): SeaTransport;
    {
        return new SeaTransport2;
    }
}
```
Và mỗi một loại phương thức vận chuyển sẽ lại thực hiện các phương thức chung thông qua 1 interface.

```php
namespace Modules\Services\Logistic;

interface RoadTransportInterface
{
    public function roadDeliver();
}

class RoadTransport1 implements RoadTransportInterface
{
    public function roadDeliver()
    {
        // todo something
    }
}

class RoadTransport2 implements RoadTransportInterface
{
    public function roadDeliver()
    {
        // todo something
    }
} 

interface SeaTransportInterface
{
    public function seaDeliver();
}

class SeaTransport1 implements SeaTransportInterface
{
    public function seaDeliver()
    {
        // todo something
    }
}

class SeaTransport2 implements SeaTransportInterface
{
    public function seaDeliver()
    {
        // todo something
    }
} 

```
