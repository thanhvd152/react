# Tai_lieu_dev_mobile
Tài liệu được ghi chép trong quá trình dev
- Tạo react-native vertion
+ react-native init TestUI  --version 0.51.0
- replace só thành dạng giá tiền :
+ input text  : =>    let bill = '';
            if (!bill) bill = text.replace(new RegExp('\\.', 'g'), '');
            this.setState({ text: bill })
value = this.state.text.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".")

+ nếu là string  => string.replace(/\B(?=(\d{3})+(?!\d))/g, ".")
