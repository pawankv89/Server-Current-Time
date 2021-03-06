# Server-Current-Time

## We are getting server time and convert accordingly our country.

Added Some screens here.

![](https://github.com/pawankv89/Server-Current-Time/blob/master/images/screen_1.png)

## Usage

#### Controller

``` swift 

import UIKit
import Foundation

class ViewController: UIViewController {
    
    @IBOutlet weak var serverDomain: UILabel!
    @IBOutlet weak var serverTime: UILabel!
    @IBOutlet weak var local: UILabel!

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        
        self.returnServerTime { (getResDate) -> Void in
            let dFormatter = DateFormatter()
            dFormatter.dateFormat = "yyyy/MM/dd HH:mm:ss"
            dFormatter.timeZone = NSTimeZone(abbreviation: "india") as TimeZone?
            let currentTime = dFormatter.string(from: getResDate! as Date)
            print("India Time:- ", currentTime)
            DispatchQueue.main.sync {
            self.local.text! = currentTime
            }
            
        }//
    }

    func returnServerTime(completionHandler:@escaping (_ getResDate: NSDate?) -> Void){
        let stringurl = "http://" + "google.com"
        self.serverDomain.text! = stringurl
        let url = NSURL(string: stringurl)
        let task = URLSession.shared.dataTask(with: url! as URL) {(data, response, error) in
            let httpResponse = response as? HTTPURLResponse
            if let contentType = httpResponse?.allHeaderFields["Date"] as? String {
                print("Server Date:- ", contentType)
                DispatchQueue.main.sync {
                     self.serverTime.text! = contentType
                }
               
                let dFormatter = DateFormatter()
                dFormatter.dateFormat = "EEE, dd MMM yyyy HH:mm:ss z"
                let serverTime = dFormatter.date(from: contentType)
                completionHandler(serverTime! as NSDate)
            }else{
                return
            }
        }
        task.resume()
    }
}


```

## License

This code is distributed under the terms and conditions of the [MIT license](LICENSE).

## Change-log

A brief summary of each this release can be found in the [CHANGELOG](CHANGELOG.mdown). 


