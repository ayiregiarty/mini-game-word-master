<template>
    <div>
        <input type="text" v-model="filter.username" placeholder="Search by Username..." />
        <input type="text" v-model="filter.role" placeholder="Search by Role..." />
        <input type="text" v-model="filter.status" placeholder="Search by Status..." />
        <table>
            <thead>
                <tr>
                    <th @click="sort('username')">Username</th>
                    <th @click="sort('role')">Role</th>
                    <th @click="sort('status')">Status</th>
                    <th @click="sort('lastlogin')">Last Logged In</th>
                    <th @click="sort('lastmodified')">Last Modified</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="user in filteredUser" :key="user.username">
                    <td>{{ user.username }}</td>
                    <td>{{ user.role }}</td>
                    <td>{{ user.status }}</td>
                    <td>{{ user.convLastLogin }}</td>
                    <td>{{ user.convLastModified }}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>
<script>
export default {
    data() {
        return {
            users: [],
            filter: {
                username: '',
                role: '',
                status: '',
            },
            sortKey: '',
            sortOrder: 'asc',
        }
    },
    created() {
        this.fetchUser();
    },
    methods: {
        async fetchUser() {
            const response = await fetch('/user.json');
            this.users = await response.json();
            console.log(this.users)
        },
        sort(key) {
            this.sortOrder = (this.sortKey === key && this.sortOrder === 'asc') ? 'desc' : 'asc';
            this.sortKey = key;
        },
    },
    computed: {
        filteredUser() {
            return this.users
            .filter(user => 
                user.username.toLowerCase().includes(this.filter.username.toLowerCase()) &&
                user.role.toLowerCase().includes(this.filter.role.toLowerCase()) &&
                user.status.toLowerCase().includes(this.filter.status.toLowerCase())
            )
            .map(user => ({
                ...user,
                convLastLogin: new Date(+user.lastlogin).toISOString(),
                convLastModified: new Date(+user.lastmodified).toISOString(),
            })) 
            .sort((a, b) => {
                if (this.sortKey) {
                    const modif = this.sortOrder === 'asc' ? 1 : -1;
                    if(a[this.sortKey] < b[this.sortKey]) {
                        return -1 * modif;
                    }
                    if(a[this.sortKey] > b[this.sortKey]) {
                        return 1 * modif;
                    }
                }
                return 0;
            })
        }
    }
}
</script>
<style scoped>
    table {
        width: 100%;
        border-collapse: collapse;
    }
    th, td {
        border: 1px solid #ddd;
        padding: 4px;
    }
    th {
        cursor: pointer;
        background: #d9d9d9;
    }
    input {
        width: 30%;
        display: inline-block;
        margin-bottom: 20px;
        margin-right: 10px;
        height: 20px;
    }
</style>