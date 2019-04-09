```go
// 通过tag校验单个字段
type ExampleStruct struct {
    Name  string `valid:"skipempty;alpha"`
    Age   int    `valid:"range(1,100)"`
    Email string `valid:"email"`
}

// 通过实现Validator接口，实现跨字段校验
func (v *ExampleStruct) Validate() error {
    if v.Name == "" && v.Email == "" {
        return errors.New("name and email should not both empty")
    }
    return nil
}
```

## 注册自定义的Validator
```go
govalidator.TagValidatorMap.RegisterValidateFunc("sortfields", validateSortFields)
govalidator.TagValidatorMap.RegisterValidator("example", ExampleValidator{})
```

## 关于dive的说明
对于pointer,slice,array,map等类型的校验，默认校验其本身。
如果要校验pointer指向的对象，或slice、array、map内的元素的值，需要使用dive
示例如下:
```go
// 校验Name指针不为空，且其指向的字符串为alpha字符集
type PointerFieldStruct struct {
    Name *string `valid:"required;dive;alpha"`
}
```

## 支持的Tag validator
```go
"email":              IsEmail,
"url":                IsURL,
"alpha":              IsAlpha,
"alphanum":           IsAlphanumeric,
"numeric":            IsNumeric,
"lowercase":          IsLowerCase,
"uppercase":          IsUpperCase,
"int":                IsInt,
"float":              IsFloat,
"empty":              IsEmpty,
"json":               IsJSON,
"ascii":              IsASCII,
"hash":               IsHash,
"printableascii":     IsPrintableASCII,
"base64":             IsBase64,
"ip":                 IsIP,
"port":               IsPort,
"ipv4":               IsIPv4,
"ipv6":               IsIPv6,
"mac":                IsMAC,
"latitude":           IsLatitude,
"longitude":          IsLongitude,
"rfc3339":            IsRFC3339,
"rfc3339WithoutZone": IsRFC3339WithoutZone,
"ISO4217":            IsISO4217,
"required":           Required,
"in":                 IsIn,
"min":                Min,
"max":                Max,
"range":              Range,
"length":             Length,
"skipempty":          SkipEmpty,
"regex":              RegEx,
"dive":              // dive into slice, array, ptr, map
```
