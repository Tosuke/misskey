<template>
<mk-ui>
	<b-card header="アプリケーションの作成">
		<b-form @submit.prevent="onSubmit" autocomplete="off">
			<b-form-group label="アプリケーション名" description="あなたのアプリの名称。">
				<b-form-input v-model="name" type="text" placeholder="ex) Misskey for iOS" autocomplete="off" required/>
			</b-form-group>
			<b-form-group label="アプリの概要" description="あなたのアプリの簡単な説明や紹介。">
				<b-textarea v-model="description" placeholder="ex) Misskey iOSクライアント。" autocomplete="off" required></b-textarea>
			</b-form-group>
			<b-form-group label="コールバックURL (オプション)" description="ユーザーが認証フォームで認証した際にリダイレクトするURLを設定できます。">
				<b-input v-model="cb" type="url" placeholder="ex) https://your.app.example.com/callback.php" autocomplete="off"/>
			</b-form-group>
			<b-card header="権限">
				<b-form-group description="ここで要求した機能だけがAPIからアクセスできます。">
					<b-alert show variant="warning">%fa:exclamation-triangle%アプリ作成後も変更できますが、新たな権限を付与する場合、その時点で関連付けられているユーザーキーはすべて無効になります。</b-alert>
					<b-form-checkbox-group v-model="permission" stacked>
						<b-form-checkbox value="account-read">アカウントの情報を見る。</b-form-checkbox>
						<b-form-checkbox value="account-write">アカウントの情報を操作する。</b-form-checkbox>
						<b-form-checkbox value="note-write">投稿する。</b-form-checkbox>
						<b-form-checkbox value="reaction-write">リアクションしたりリアクションをキャンセルする。</b-form-checkbox>
						<b-form-checkbox value="following-write">フォローしたりフォロー解除する。</b-form-checkbox>
						<b-form-checkbox value="drive-read">ドライブを見る。</b-form-checkbox>
						<b-form-checkbox value="drive-write">ドライブを操作する。</b-form-checkbox>
						<b-form-checkbox value="notification-read">通知を見る。</b-form-checkbox>
						<b-form-checkbox value="notification-write">通知を操作する。</b-form-checkbox>
					</b-form-checkbox-group>
				</b-form-group>
			</b-card>
			<hr>
			<b-button type="submit" variant="primary">アプリ作成</b-button>
		</b-form>
	</b-card>
</mk-ui>
</template>

<script lang="ts">
import Vue from 'vue';
export default Vue.extend({
	data() {
		return {
			name: '',
			description: '',
			cb: '',
			nidState: null,
			permission: []
		};
	},
	methods: {
		onSubmit() {
			(this as any).api('app/create', {
				name: this.name,
				description: this.description,
				callbackUrl: this.cb,
				permission: this.permission
			}).then(() => {
				location.href = '/dev/apps';
			}).catch(() => {
				alert('アプリの作成に失敗しました。再度お試しください。');
			});
		}
	}
});
</script>
