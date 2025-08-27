// تطبيق "النسر للجملة" - React Native (Expo)
// إضافة تسجيل دخول برقم الموبايل والعنوان
// مبدئية (MVP) سهلة التنصيب ورفعها على GitHub
// يفضل تسمية الملف: App.js (الافتراضي في Expo)
// خطوات التشغيل:
// 1. npm install -g expo-cli
// 2. expo init nesr-app --template blank
// 3. انسخ الكود في App.js
// 4. expo start

import React, { useState } from 'react';
import { View, Text, TouchableOpacity, FlatList, SafeAreaView, StyleSheet, Alert, TextInput } from 'react-native';

export default function App() {
  const [loggedIn, setLoggedIn] = useState(false);
  const [phone, setPhone] = useState('');
  const [address, setAddress] = useState('');
  const [cart, setCart] = useState([]);

  const products = [
    { id: '1', name: 'زيت طعام', price: 50 },
    { id: '2', name: 'مسحوق غسيل', price: 100 },
    { id: '3', name: 'مكرونة', price: 25 },
    { id: '4', name: 'أرز', price: 40 },
  ];

  const addToCart = (item) => {
    setCart([...cart, item]);
  };

  const checkout = () => {
    if (cart.length === 0) {
      Alert.alert("السلة فارغة", "أضف منتجات أولاً");
      return;
    }
    Alert.alert(
      "تم الطلب ✅",
      `رقم الموبايل: ${phone}\nالعنوان: ${address}\nالتوصيل: مندوب شحن\nالدفع: عند الاستلام`
    );
    setCart([]);
  };

  if (!loggedIn) {
    return (
      <SafeAreaView style={styles.container}>
        <Text style={styles.header}>🦅 النسر للجملة</Text>
        <Text style={styles.subHeader}>تسجيل الدخول</Text>

        <Text>رقم الموبايل:</Text>
        <TextInput
          style={styles.input}
          placeholder="0123456789"
          keyboardType="phone-pad"
          value={phone}
          onChangeText={setPhone}
        />

        <Text>العنوان:</Text>
        <TextInput
          style={styles.input}
          placeholder="اكتب عنوانك"
          value={address}
          onChangeText={setAddress}
        />

        <TouchableOpacity
          style={styles.loginBtn}
          onPress={() => {
            if (!phone || !address) {
              Alert.alert("خطأ", "من فضلك أدخل رقم الموبايل والعنوان");
              return;
            }
            setLoggedIn(true);
          }}
        >
          <Text style={styles.loginText}>دخول</Text>
        </TouchableOpacity>
      </SafeAreaView>
    );
  }

  return (
    <SafeAreaView style={styles.container}>
      <Text style={styles.header}>🦅 النسر للجملة</Text>

      <Text style={styles.subHeader}>المنتجات</Text>
      <FlatList
        data={products}
        keyExtractor={(item) => item.id}
        renderItem={({ item }) => (
          <View style={styles.product}>
            <Text>{item.name} - {item.price} ج.م</Text>
            <TouchableOpacity style={styles.addBtn} onPress={() => addToCart(item)}>
              <Text style={styles.btnText}>+ أضف</Text>
            </TouchableOpacity>
          </View>
        )}
      />

      <Text style={styles.subHeader}>🛒 السلة ({cart.length})</Text>
      {cart.map((item, index) => (
        <Text key={index}>- {item.name}</Text>
      ))}

      <TouchableOpacity style={styles.checkoutBtn} onPress={checkout}>
        <Text style={styles.checkoutText}>اطلب الآن (الدفع عند الاستلام)</Text>
      </TouchableOpacity>
    </SafeAreaView>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, backgroundColor: '#fff', padding
