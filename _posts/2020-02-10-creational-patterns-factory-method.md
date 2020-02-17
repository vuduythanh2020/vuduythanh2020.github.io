---
layout: post
title: Creational Patterns - Factory Method
---

Factory Method cung cấp một interface để khởi tạo các đối tượng từ một lớp cha. Những đối tượng này sẽ được tuỳ chỉnh lại ở các lớp con.

Bài toán
-------------

Hãy tưởng tượng bạn có một ứng dụng quản lý vận chuyển hàng hoá. Ban đầu, ứng dụng của bạn chỉ vận chuyển bằng xe tải. Tất cả logic bạn sẽ viết trong một lớp Trunk. Nhưng sau đó ứng dụng của bạn trở nên quen thuộc và được nhiều người biết đến. Rồi một ngày có rất nhiều các công ty vận chuyển đường biển muốn hợp tác để được đưa lên ứng dụng của bạn. Một tin tốt lành nhưng việc tích hợp vào ứng dụng sẽ phải sửa lại code như thế nào? 

Nếu bạn sửa tiếp logic trong lớp Trunk thì có được không? Rồi một ngày nào đó thêm công ty vận chuyển hàng không hay nhiều công ty vận chuyển khác nữa muốn làm việc với bạn thì phải làm sao? Nếu cứ cố gắng sửa trong lớp Trunk thì chắc chắn code của bạn sẽ rất rối rắm khi phải đan xen rất nhiều logic xử lý khác nhau...

Giải pháp
------------

Bây giờ chúng ta hãy phân tích lại từ đầu để tạo 1 kiến trúc cho bài toán trên nhé.

Hệ thống vận chuyển (logistics) sẽ có nhiều các phương thức vận chuyển. Có thể là đường bộ, đường biển, đường hàng không... Trong bài toán chúng ta đang có 2 phương thức là đường bộ và đường biển.
Vậy chúng ta sẽ tạo ra 3 lớp Logistic, Road Logistic và Sea Logistic:
![Alt text](https://refactoring.guru/images/patterns/diagrams/factory-method/solution1.png)

```php
<?php

namespace Modules\Services\Logistic;

/**
 * Lớp Logistic sẽ định nghĩa phương thức createTransport() trả về đối tượng vận chuyển (Vehicle) cụ thể.
 * Các lớp kế thừa Logistic sẽ thực thi lại phương thức createTransport()
 */
abstract class Logistic
{
 
    abstract public function createTransport(): Vehicle;

    /**
     * Trong lớp Logistic này sẽ chứa những logic xử lý cốt lõi 
     * Ví dụ một phương tiện vận chuyển sẽ thực thi hành động vận chuyển (deliver)
     */
    public function planDeliver()
    {
        // Call the factory method to create a Vehicle object.
        $vehicle = $this->createTransport();
        // Now, use the vehicle
        $vehicle->deliver();
    }
}
```

```php
<?php

namespace Modules\Services\Logistic;

/**
 * RoadLogistic kế thừa Logistic
 * phương thức createTransport trả về phương tiện vận chuyển cụ thể của đường bộ (Truck)   
 */
class RoadLogistic extends Logistic
{
    public function createTransport(): Vehicle;
    {
        return new Truck();
    }
}
```

```php
<?php

namespace Modules\Services\Logistic;

/**
 * RoadLogistic kế thừa Logistic
 * phương thức createTransport trả về phương tiện vận chuyển cụ thể của đường biển (Ship)   
 */
class SeaLogistic extends Logistic
{
    public function createTransport(): Vehicle;
    {
        return new Ship();
    }
}
```

Okie như vậy khi cần thực hiện kế hoạch vận chuyển chúng ta sẽ viết như sau

```php
<?php

(new RoadLogistic)->planDeliver();

// Or

(new SeaLogistic)->planDeliver();
```

Tất nhiên ở đây các lớp Trunk và Ship phải có phương thức deliver(). Khi các lớp sử dụng chung các phương thức thì chúng ta sẽ sử dụng 
interface. Chúng ta sẽ có code cụ thể như sau:

```php
<?php

namespace Modules\Services\Logistic;

interface Transport
{
    public function deliver();
}

class Trunk implements Transport 
{   
    public function deliver() {
        // to do for Trunk transport
    }
}

class Ship implements Transport 
{   
    public function deliver() {
        // to do for Ship transport
    }
}

```

