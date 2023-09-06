<template>
    <v-form ref="LoginForm" :lazy-validation="lazy">
        <v-card width="50%" height="100%">
            <v-card-title>เข้าสู่ระบบ</v-card-title>
        <v-card-text>
            <v-row dense>
                <v-col cols="12" align="start">
                    <span>ชื่อผู้ใช้งาน: </span><br/>
                    <v-text-field v-model="username" rounded outlined dense :rules="Rules.usernameRules"></v-text-field>
                </v-col>
            </v-row>
            <v-row dense>
                <v-col cols="12" align="start">
                    <span>รหัสผ่าน: </span><br/>
                    <v-text-field v-model="password" rounded outlined dense :rules="Rules.passwordRules"></v-text-field>
                </v-col>
            </v-row>
        </v-card-text>
        <v-card-actions>
            <v-btn block rounded @click="login()">เข้าสู่ระบบ</v-btn>
        </v-card-actions>

        </v-card>
    </v-form>
</template>

<script>
export default {

}
</script>
