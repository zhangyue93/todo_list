<template>

	<div class="t-body">
		
		<v-banner
		icon="mdi-account large "
		class="font-weight-black text-h5 cyan--text text--darken-3"
		>作業状況一覧</v-banner>  
		<p></p>
		<p class="font-weight-black">
		<label v-for="(label,key) in options" :key="key">
			<input type="radio"
				v-model="current"
				v-bind:value="label.value">
				{{label.label }}
		</label>
		（{{ computedTodos.length }} 件を表示）</p>
		
		
		
		
		<v-simple-table 
		dense
		fixed-header
		height="300px"
		
		>
			<thead>
				<tr>
					<th class="text-center subtitle-2 font-weight-black">ID</th>
					<th class="text-center subtitle-2 font-weight-black">作業</th>
					<th class="text-center subtitle-2 font-weight-black">コメント</th>
					<th class="text-center subtitle-2 font-weight-black">状態</th>
					<th class="text-center subtitle-2 font-weight-black">処理</th>
				</tr>
				
				
			</thead>
			<!--step5-->
			<tbody>
				<tr v-for="item in computedTodos" v-bind:key="item.id">
					<th　class="text-center">{{ item.id }}</th>
					<td　class="text-center">{{ item.comment }}</td>
					<td class="text-center">
						<v-text-field placeholder="ここに入力してください"></v-text-field>
					</td>
					<td class="text-center">
						<v-btn v-on:click="doChangeState(item)"
						
						small
						>
						{{ labels[item.state] }}
						</v-btn>
					</td>
					<td class="text-center">
			
						<v-btn v-on:click.ctrl="doRemove(item)" 
						small
						>
							<v-icon
							color="red"
							small
							>mdi-delete</v-icon>
						</v-btn>
					</td>
					</tr>
			</tbody> 
		</v-simple-table>
		<p></p>
		<p class="red--text text--darken-1"
		>※削除ボタンはコントロールキーを押しながらクリックして下さい</p>
		
		<v-banner 
		icon="mdi-send large "
		class="font-weight-black text-h5 cyan--text text--darken-3"
		>新しい作業の追加</v-banner>  
		<p></p>
		<v-spacer></v-spacer>
		<v-form class="add-form　text-h6" v-on:submit.prevent="doAdd"> 
		
			<input style="height: 35px;width: 600px;" type="text" ref="comment">
			
			<v-col
			cols="4">
				<v-text-field
					:counter="20"
					:rules="[v => !!v || 'Item is required']"
					label="作業"
				></v-text-field>
			</v-col>
			
			<v-btn type="submit"
			small
			>
				<v-icon
				small
				>mdi-plus</v-icon>
			</v-btn>
		</v-form>
		</div>
		

</template>

<script>
	var STORAGE_KEY = 'todos-vuejs-demo'
	var todoStorage = {
		fetch: function () {
		var todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]')
		todos.forEach(function (todo, index) {
			todo.id = index
		})
		todoStorage.uid = todos.length
		return todos
		},
		save: function (todos) {
		localStorage.setItem(STORAGE_KEY, JSON.stringify(todos))
		}
	}
	
	export default{
		name:"Tbody",
		data(){
			return{
				todos: [],
				
				current: -1,
				
				options: [
					{ value: -1, label: 'すべて' },
					{ value: 0, label: '作業中' },
					{ value: 1, label: '完了' }
				],
				
			}
		},
		
		
		computed:{
			labels:function(){
				return this.options.reduce(function (a, b) {
					return Object.assign(a, { [b.value]: b.label })
				}, {})
				// キーから見つけやすいように、次のように加工したデータを作成
				// {0: '作業中', 1: '完了', -1: 'すべて'}
				},
				
			computedTodos:function () {
				return this.todos.filter(function (el) {
				return this.current < 0 ? true : this.current === el.state
				}, this)
			}
		},
		
		watch: {
			// オプションを使う場合はオブジェクト形式にする
			todos: {
			// 引数はウォッチしているプロパティの変更後の値
			handler: function (todos) {
				todoStorage.save(todos)
			},
			// deep オプションでネストしているデータも監視できる
			deep: true
			}
		},
		created() {
			// インスタンス作成時に自動的に fetch() する
			this.todos = todoStorage.fetch()
		},
		methods: {
			
		
			// ★STEP7 ToDo 追加の処理
			doAdd: function() {
			// ref で名前を付けておいた要素を参照
				var comment = this.$refs.comment
			// 入力がなければ何もしないで return
				if (!comment.value.length) {
					return
				}
			// { 新しいID, コメント, 作業状態 }
			// というオブジェクトを現在の todos リストへ push
			// 作業状態「state」はデフォルト「作業中=0」で作成
				this.todos.push({
					id: todoStorage.uid++,
					comment: comment.value,
					state: 0
				})
				
				//alert(this.todos.length)
			// フォーム要素を空にする
				comment.value = ''
				},
		
			// ★STEP10 状態変更の処理
			doChangeState: function (item) {
			item.state = !item.state ? 1 : 0
			},
		
			// ★STEP10 削除の処理
			doRemove: function (item) {
			var index = this.todos.indexOf(item)
			this.todos.splice(index, 1)
			}

		}
	}
</script>

<style>
	
</style>