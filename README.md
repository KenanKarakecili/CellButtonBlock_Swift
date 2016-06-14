# KKCellButtonBlock_Swift

It's very often we add a button in a UITableViewCell.

But to handle its action is not so handy.

So I decided to write a handler class that can store every cell button and its action in UITableViewCells.
```
class KKCellButtonBlock {
  
  var actionDic: [String: Void -> Void] = [:]
  
  func setActionForButton(button: UIButton, action: Void -> Void) {
    let uniqueId = "\(unsafeAddressOf(button))"
    actionDic[uniqueId] = action
    button.addTarget(self, action: #selector(didTapButton(_:)), forControlEvents: .TouchUpInside)
  }
  
  @objc func didTapButton(sender: UIButton) {
    let uniqueId = "\(unsafeAddressOf(sender))"
    let action = actionDic[uniqueId]
    if let myAction = action {
      myAction()
    }
  }
  
}
```

#Usage

Step1:
Create an instance in ViewController class.

```
let cellButtonBlock = KKCellButtonBlock()
```

Step2:
In UITableView's cellForRow method, add your action inside the closure.
```
func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
  let cell ...
  cellButtonBlock.setActionForButton(cell!.cellApproveButton) { () in
    self.anAction()
  }
  return cell
}
```
