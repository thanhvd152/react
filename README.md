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

/////////////////////////////
Tip js ==> https://viblo.asia/p/43-thu-thuat-hay-va-huu-ich-voi-javascript-ByEZk9Rg5Q0
/////////////////////////////customtab//////////////////////////

import React, { Component } from 'react';
import { Platform, StyleSheet, Text, View, TouchableOpacity, Animated, Dimensions, PanResponder, ImageBackground } from 'react-native';
import api from '../api';

export default class CustomDrawer extends Component {
    constructor(props) {
        super(props)
    }
    isDrawOpent = false

    scroll = new Animated.Value(0)

    opentDrawer = () => {
        Animated.timing(this.scroll, {
            toValue: 150,
        }).start(() => {
            this.isDrawOpent = true
        })

    }
    closeDrawer = () => {
        Animated.timing(this.scroll, {
            toValue: 0,
        }).start(() => {
            this.isDrawOpent = false
        })

    }



    pan = PanResponder.create({
        // Ask to be the responder:
        onStartShouldSetPanResponder: (evt, gestureState) => false,
        onStartShouldSetPanResponderCapture: (evt, gestureState) => false,
        onMoveShouldSetPanResponder: (evt, gestureState) => true,
        onMoveShouldSetPanResponderCapture: (evt, gestureState) => false,
        onPanResponderGrant: (evt, gestureState) => {

        },
        onPanResponderMove: (e, gestureState) => {
            if (!this.isDrawOpent && gestureState.x0 < 10) {
                Animated.event(
                    [null,
                        { dx: this.scroll }
                    ])(e, gestureState)
            } 
        },
        onResponderReject: (evt) => {
            alert('a')
        },
        onPanResponderTerminationRequest: (evt, gestureState) => false,
        onPanResponderRelease: (evt, gestureState) => {
            console.log(gestureState.moveX)
            if (gestureState.dx > Dimensions.get('window').width / 20 && gestureState.x0 < 10) {
                Animated.timing(this.scroll, {
                    toValue: 150,
                }).start()
                this.isDrawOpent = true
            } else {
                Animated.timing(this.scroll, {
                    toValue: 0,
                }).start()
                this.isDrawOpent = false
            }
        },
        onPanResponderTerminate: (evt, gestureState) => {

        },
        onShouldBlockNativeResponder: (evt, gestureState) => {
            return false;
        }
    });




    render() {
        let rotateLeft = this.scroll.interpolate({
            inputRange: [0, 150],
            outputRange: ['-40deg', '0deg'],
            extrapolate: 'clamp'
        })
        let rotateRight = this.scroll.interpolate({
            inputRange: [0, 150],
            outputRange: ['0deg', '-65deg'],
            extrapolate: 'clamp'
        })
        let translateRight = this.scroll.interpolate({
            inputRange: [0, 150],
            outputRange: [0, Dimensions.get('window').width / 1.3],
            extrapolate: 'clamp'
        })
        let scaleRight = this.scroll.interpolate({
            inputRange: [0, 150],
            outputRange: [1, 0.7],
            extrapolate: 'clamp'
        })

        let translateLeft = this.scroll.interpolate({
            inputRange: [0, 150],
            outputRange: [-Dimensions.get('window').width / 2, 0],
            extrapolate: 'clamp'
        })
        let opacityLeft = this.scroll.interpolate({
            inputRange: [0, 150],
            outputRange: [0, 1],
            extrapolate: 'clamp'
        })

        let zIndexTouch = this.scroll.interpolate({
            inputRange: [0, 150],
            outputRange: [-1, 2],
            extrapolate: 'clamp'
        })

        let ContentScreen = this.props.contentScreen;
        let Menu = this.props.menu
        return (
            <View {...this.pan.panHandlers} style={{ flex: 1, backgroundColor: 'pink' }}
            >
                <ImageBackground style={{ flex: 1 }} source={require('../img/bg_mn.png')}>

                    <Animated.View style={{
                        flex: 1,
                        backgroundColor: 'transparent',
                        position: 'absolute',
                        zIndex: zIndexTouch,
                        transform: [{ rotateY: rotateRight }, { translateX: translateRight }]
                    }}>
                        <View style={{ width: api.getRealDeviceWidth(), height: api.getRealDeviceHeight() }}>
                            <TouchableOpacity
                                activeOpacity={1}
                                onPress={() => {
                                    if (this.isDrawOpent) {
                                        Animated.timing(this.scroll, {
                                            toValue: 0,
                                        }).start(() => { this.isDrawOpent = false })
                                    }
                                }}
                                style={{
                                    flex: 1,
                                    backgroundColor: 'transparent'
                                }}>
                            </TouchableOpacity>
                        </View>
                    </Animated.View>

                    <Animated.View style={{
                        flex: 1,
                        zIndex: 1,
                        transform: [{ rotateY: rotateRight }, { translateX: translateRight }, { scaleY: scaleRight }]
                    }}>
                        <View style={{ width: api.getRealDeviceWidth(), height: api.getRealDeviceHeight() }}>
                            <ContentScreen screenProps={{ opentDrawer: this.opentDrawer }} navigation={this.props.navigation} />
                        </View>
                    </Animated.View>
                    <Animated.View style={{
                        height: Dimensions.get('window').height,
                        position: 'absolute',
                        zIndex: 0,
                        opacity: opacityLeft,
                        width: Dimensions.get('window').width / 1.7,
                        transform: [{ rotateY: rotateLeft }, { translateX: translateLeft }]
                    }}>
                        <View style={{ flex: 1, zIndex: 0 }}>
                            <Menu navigation={this.props.navigation} />
                        </View>


                    </Animated.View>
                </ImageBackground>
            </View>
        );
    }
}

const styles = StyleSheet.create({
    container: {

    },
    welcome: {
        fontSize: 20,
        textAlign: 'center',
        margin: 10,
    },
    instructions: {
        textAlign: 'center',
        color: '#333333',
        marginBottom: 5,
    },
});


/////////tabbar//////////////
const HomeWrapper = ({ navigation }) => {
    console.log('nav', navigation);
    return (
        <CustomDrawer navigation={navigation} contentScreen={BottomTab} menu={Navdrawer} />
    );
}
HomeWrapper.router = BottomTab.router;
