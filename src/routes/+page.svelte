<script>
  import { redirect } from '@sveltejs/kit';
  
  // Redirect to the first component by default
  throw redirect(302, '/instagram-aspect-ratios');
</script>