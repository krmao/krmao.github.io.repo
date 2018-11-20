---
title: 移动端_IOS_SWIFT_4_0_ALAMOFILE_同步执行
date: 2018-11-19 18:29:49
categories: [技术]
tags: [移动端,IOS]
---

```
    static func downloadByFullURLSync<T: Codable>(url: String, timeoutInterval: Double = 60) -> T? {
        CXLogUtil.e("download 0 sync \(Thread.currentThread())")
        let semaphore = DispatchSemaphore.init(value: 0)

        var responseModel: T?

        let start = Date()
        Alamofire.download(URLRequest(url: NSURL.init(string: url)! as URL, timeoutInterval: timeoutInterval))
            .responseJSON(queue: DispatchQueue.global(qos: DispatchQoS.QoSClass.background), options: .allowFragments, completionHandler:  { dataResponse in
                    CXLogUtil.e("download 2 sync \(Thread.currentThread())")

                    print("\n\n>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>")
                    print("request url: ", dataResponse.request?.url ?? "")
                    print("\nrequest headers:\n")
                    dataResponse.request?.allHTTPHeaderFields?.forEach { key, value in
                        print("\t", key, value)
                    }
                    print("\n<<<<<<<<<<---------->>>>>>>>>>\n")
                    print("response headers:\n")
                    dataResponse.response?.allHeaderFields.forEach { key, value in
                        print("\t", key, value)
                    }

                    print("\nresponse body:\n\n\t\(dataResponse.result.value ?? "")\n")

                    let ms = round(Date().timeIntervalSince(start) * 1000)
                    print("<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< 耗时: \(ms) ms \n\n")
                    if (dataResponse.result.isSuccess && dataResponse.result.value != nil) {
                        responseModel = CXJsonUtil.parse(T.self, withJSONObject: dataResponse.result.value ?? "")
                        print("\nresponse success:\n")
                    } else {
                        responseModel = nil
                        let failureMessage = dataResponse.result.error?.localizedDescription.lowercased() ?? "网络错误"
                        print("\nresponse failure:\n\n\t\(failureMessage)\n")
                    }
                    semaphore.signal()
                })

        _ = semaphore.wait(timeout: DispatchTime.distantFuture)
        CXLogUtil.e("download 1 sync \(Thread.currentThread())")
        return responseModel
    }
```
