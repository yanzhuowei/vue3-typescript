  <template>
  <div class="create-post-page">
    <h4>新建文章</h4>
    <validate-form @form-submit="onFormSubmit">
      <div class="mb-3">
        <label class="form-label">文章标题：</label>
        <validate-input :rules="titleRules" v-model="titleVal" placeholder="请输入文章标题" type="text" />
      </div>
      <div class="mb-3">
        <label class="form-label">文章详情：</label>
        <validate-input rows="10" tag="textarea" placeholder="请输入文章详情" :rules="contentRules" v-model="contentVal" />
      </div>
      <template #submit>
        <button class="btn btn-primary btn-large">发表文章</button>
      </template>
    </validate-form>
  </div>
</template>
<script lang="ts">
import { defineComponent, ref } from 'vue'
import ValidateInput, { RulesProp } from '../components/ValidateInput.vue'
import ValidateForm from '../components/ValidateForm.vue'
import { useStore } from 'vuex'
import { GlobalDataProps, PostProps } from '@/store'
import { useRouter } from 'vue-router'
export default defineComponent({
  name: 'Login',
  components: {
    ValidateInput,
    ValidateForm
  },
  setup () {
    const store = useStore<GlobalDataProps>()
    const router = useRouter()
    const titleRules: RulesProp = [
      { type: 'required', message: '文章标题不能为空' }
    ]
    const contentVal = ref('')
    const titleVal = ref('')
    const contentRules: RulesProp = [
      { type: 'required', message: '文章详情不能为空' }
    ]

    const onFormSubmit = (result: boolean) => {
      if (result) {
        const { columnId } = store.state.user
        if (columnId) {
          const newPost: PostProps = {
            id: 1,
            title: titleVal.value,
            content: contentVal.value,
            createdAt: new Date().toLocaleString(),
            columnId: columnId
          }
          store.commit('createPost', newPost)
          router.push({ name: 'column', params: { id: columnId } })
        }
      }
    }

    return {
      titleVal,
      titleRules,
      contentVal,
      contentRules,
      onFormSubmit
    }
  }
})
</script>
