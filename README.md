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


<Tabs tabBarUnderlineStyle={{ borderBottomWidth: 2, borderBottomColor: '#00CC99', height: 2 }} renderTabBar={() => <ScrollableTab style={{ height: 40 }} backgroundColor={'#fff'} />}>
                        {arr.map(item => {
                            return (
                                <Tab key={item.id} tabStyle={{ backgroundColor: '#fff' }} activeTabStyle={{ backgroundColor: '#fff' }} textStyle={{ color: '#333333' }} activeTextStyle={{ color: '#00CC99' }} heading={item.name}>
                                    <ListPromotion />
                                </Tab>
                            )
                        })}

                    </Tabs>
patch-package vá lỗi node_module
