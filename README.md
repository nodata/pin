# pin
Syntax sugar for the verbose NSLayoutConstraint api

![pins](https://user-images.githubusercontent.com/1578003/151350111-a972f743-7b37-4f2e-9846-0972a70304a5.png)

This was built a long time ago in various guises but finally released as part of the Mercari - Fractal Open Sourcing.. however it's worth having it in own package as it has stand alone value.

```
        // Pinning to self

        let aSubview = UIView()
        addSubview(aSubview)
        aSubview.pin(to: self, [.leading, .top, .width, .height])
                
        // or

        aSubview.pin(to: self, [.centerX, .centerY, .width, .height])

        // or just

        aSubview.pin(to: self)

        // Pinning with padding

        aSubview.pin(to: self, [.leading(10.0), .top(10.0), .width(-20.0), .height(-20.0)])

```

```
        let firstView = UIView()
        addSubview(firstView)

        let secondView = UIView()
        addSubview(secondView)

        // Pinning one view to various different views and constant width / heights

        firstView.pin(to: self, [.leading(10.0), .centerY])
        firstView.pin([.width(100.0), .height(100.0)])

        secondView.pin(to: self, [.centerY])
        secondView.pin(to: firstView, [.rightOf(10.0), .width, .height])

        // Above Pins can also be expressed with different syntax

        firstView.pin(to: self, [.leading(10.0), .centerY, .width(asConstant: 100.0), .height(asConstant: 100.0)])

        secondView.pin(to: [self:      [.centerY],
                            firstView: [.rightOf(10.0), .width, .height]])
```

```
    private var yConstraint: NSLayoutConstraint?
    private var someConstraints: [NSLayoutConstraint] = []

    private var wConstraint: NSLayoutConstraint?
    private var hConstraint: NSLayoutConstraint?

    func capturingConstraints() {

        let someView = UIView()
        addSubview(someView)

        yConstraint = someView.pin(to: self, [.leading, .centerY, .width, .height])[1]

        // or grab the entire array of constraints

        someConstraints = someView.pin(to: self, [.leading, .centerY, .width, .height])

        // or grab the entire array of constraints to capture two

        let constraints = someView.pin(to: self, [.leading, .centerY, .width, .height])

        wConstraint = constraints[2]
        hConstraint = constraints[3]
    }

```

```
        func withOptions() {

        let optionsSubview = UIView()
        addSubview(optionsSubview)

        // All options with their default values inside

        optionsSubview.pin(to: self, [.width(options: [.multiplier(1.0),
                                                       .priority(.almostRequired),
                                                       .isActive(true),
                                                       .relation(.equal)])])

        // Simple multiplier

        optionsSubview.pin(to: self, [.leading,
                                      .top,
                                      .width(options: [.multiplier(0.5)]),
                                      .height])

        // Multiple width pins with relation and priority

        optionsSubview.pin(to: self, [.leading(10.0),
                                      .top,
                                      .width(-20.0, options: [.relation(.lessThanOrEqual), .priority(.defaultLow)]),
                                      .width(asConstant: 100.0, options: [.relation(.greaterThanOrEqual), .priority(.defaultHigh)]),
                                      .height])

        // Multiple width pins with inactivity

        let constraints = optionsSubview.pin(to: self, [.leading(10.0),
                                                        .top,
                                                        .width(options: [.isActive(false)]),
                                                        .width(asConstant: 100.0, options: [.isActive(true)]),
                                                        .height])

        let fullWidthConstraint = constraints[2]
        let constantWidthConstraint = constraints[3]

        NSLayoutConstraint.activate([fullWidthConstraint])
        NSLayoutConstraint.deactivate([constantWidthConstraint])
    }

```

```
    func customPins() {

        let firstView = UIView()
        addSubview(firstView)
        firstView.pin(to: self, [.centerX, .centerY, .width(asConstant: 100.0), .heightToWidth])

        let secondView = UIView()
        addSubview(secondView)
        secondView.pin(to: firstView, [.leading(10.0),
                                       .custom(.centerY, to: .bottom),
                                       .width(-20.0),
                                       .height(-20.0)])
    }
```

License Copyright 2016 Anthony Smith, (no data) ltd, Licensed under the MIT License.
