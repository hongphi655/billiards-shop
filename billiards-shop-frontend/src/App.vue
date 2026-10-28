<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

// Khai báo biến reactive chứa danh sách sản phẩm
const products = ref([])
const loading = ref(true)
const errorMessage = ref('')

// Hàm gọi API lấy danh sách sản phẩm từ Spring Boot
const fetchProducts = async () => {
      try {
        const response = await axios.get('http://localhost:8080/api/products')
        products.value = response.data
      } catch (error) {
        console.error('Lỗi khi gọi API:', error)
        errorMessage.value = 'Không thể kết nối tới Backend! Hãy kiểm tra lại Spring Boot.'
      } finally {
        loading.value = false
      }
}

// Gọi API ngay khi component được tải lên
onMounted(() => {
      fetchProducts()
})
</script>

<template>
    <div style="max-width: 800px; margin: 40px auto; font-family: Arial, sans-serif;">
        <h1 style="color: #2c3e50; text-align: center;">🎱 Quản Lý Cửa Hàng Bida (Billiards Shop)</h1>

        <div v-if="loading" style="text-align: center; color: #666;">Đang tải dữ liệu từ server...</div>

        <div v-if="errorMessage" style="color: red; background: #ffe6e6; padding: 10px; border-radius: 5px; margin-bottom: 20px;">
            {{ errorMessage }}
        </div>

        <table v-if="!loading && products.length > 0" style="width: 100%; border-collapse: collapse; margin-top: 20px;">
            <thead>
                <tr style="background-color: #42b883; color: white;">
                    <th style="padding: 12px; border: 1px solid #ddd;">ID</th>
                    <th style="padding: 12px; border: 1px solid #ddd;">Tên sản phẩm</th>
                    <th style="padding: 12px; border: 1px solid #ddd;">Giá (VNĐ)</th>
                    <th style="padding: 12px; border: 1px solid #ddd;">Số lượng</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="product in products" :key="product.id" style="text-align: center;">
                    <td style="padding: 10px; border: 1px solid #ddd;">{{ product.id }}</td>
                    <td style="padding: 10px; border: 1px solid #ddd; text-align: left;">{{ product.name }}</td>
                    <td style="padding: 10px; border: 1px solid #ddd;">{{ product.price }}</td>
                    <td style="padding: 10px; border: 1px solid #ddd;">{{ product.quantity }}</td>
                </tr>
            </tbody>
        </table>

        <p v-if="!loading && products.length === 0 && !errorMessage" style="text-align: center; color: #666;">
            Chưa có sản phẩm nào trong cơ sở dữ liệu.
        </p>
    </div>
</template>