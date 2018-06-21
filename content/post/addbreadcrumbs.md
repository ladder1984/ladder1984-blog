---
title: 博客添加简单的面包屑导航
tags:
  - wordpress
id: 275
categories:
  - 网站建设
date: 2013-01-01 01:16:31
---

下面说一下方法，使用这里的代码《[WordPress Breadcrumbs Without a Plugin](http://dimox.net/wordpress-breadcrumbs-without-a-plugin/ "http://dimox.net/wordpress-breadcrumbs-without-a-plugin/")》，05.04.2012版本。本人略作修改，把1.“home”改成首页，2.不显示当前页标题，3，分隔符改为“&gt;”并在最后加个“&gt;”。代码如下：

<pre class="lang:php decode:true">function dimox_breadcrumbs() {

    /* === OPTIONS === */
    $text['home']     = 'Home'; // text for the 'Home' link
    $text['category'] = 'Archive by Category "%s"'; // text for a category page
    $text['search']   = 'Search Results for "%s" Query'; // text for a search results page
    $text['tag']      = 'Posts Tagged "%s"'; // text for a tag page
    $text['author']   = 'Articles Posted by %s'; // text for an author page
    $text['404']      = 'Error 404'; // text for the 404 page

    $show_current   = 1; // 1 - show current post/page/category title in breadcrumbs, 0 - don't show
    $show_on_home   = 0; // 1 - show breadcrumbs on the homepage, 0 - don't show
    $show_home_link = 1; // 1 - show the 'Home' link, 0 - don't show
    $show_title     = 1; // 1 - show the title for the links, 0 - don't show
    $delimiter      = ' &amp;raquo; '; // delimiter between crumbs
    $before         = '&lt;span class="current"&gt;'; // tag before the current crumb
    $after          = '&lt;/span&gt;'; // tag after the current crumb
    /* === END OF OPTIONS === */

    global $post;
    $home_link    = home_url('/');
    $link_before  = '&lt;span typeof="v:Breadcrumb"&gt;';
    $link_after   = '&lt;/span&gt;';
    $link_attr    = ' rel="v:url" property="v:title"';
    $link         = $link_before . '&lt;a' . $link_attr . ' href="%1$s"&gt;%2$s&lt;/a&gt;' . $link_after;
    $parent_id    = $parent_id_2 = $post-&gt;post_parent;
    $frontpage_id = get_option('page_on_front');

    if (is_home() || is_front_page()) {

        if ($show_on_home == 1) echo '&lt;div class="breadcrumbs"&gt;&lt;a href="' . $home_link . '"&gt;' . $text['home'] . '&lt;/a&gt;&lt;/div&gt;';

    } else {

        echo '&lt;div class="breadcrumbs" xmlns:v="http://rdf.data-vocabulary.org/#"&gt;';
        if ($show_home_link == 1) {
            echo '&lt;a href="' . $home_link . '" rel="v:url" property="v:title"&gt;' . $text['home'] . '&lt;/a&gt;';
            if ($frontpage_id == 0 || $parent_id != $frontpage_id) echo $delimiter;
        }

        if ( is_category() ) {
            $this_cat = get_category(get_query_var('cat'), false);
            if ($this_cat-&gt;parent != 0) {
                $cats = get_category_parents($this_cat-&gt;parent, TRUE, $delimiter);
                if ($show_current == 0) $cats = preg_replace("#^(.+)$delimiter$#", "$1", $cats);
                $cats = str_replace('&lt;a', $link_before . '&lt;a' . $link_attr, $cats);
                $cats = str_replace('&lt;/a&gt;', '&lt;/a&gt;' . $link_after, $cats);
                if ($show_title == 0) $cats = preg_replace('/ title="(.*?)"/', '', $cats);
                echo $cats;
            }
            if ($show_current == 1) echo $before . sprintf($text['category'], single_cat_title('', false)) . $after;

        } elseif ( is_search() ) {
            echo $before . sprintf($text['search'], get_search_query()) . $after;

        } elseif ( is_day() ) {
            echo sprintf($link, get_year_link(get_the_time('Y')), get_the_time('Y')) . $delimiter;
            echo sprintf($link, get_month_link(get_the_time('Y'),get_the_time('m')), get_the_time('F')) . $delimiter;
            echo $before . get_the_time('d') . $after;

        } elseif ( is_month() ) {
            echo sprintf($link, get_year_link(get_the_time('Y')), get_the_time('Y')) . $delimiter;
            echo $before . get_the_time('F') . $after;

        } elseif ( is_year() ) {
            echo $before . get_the_time('Y') . $after;

        } elseif ( is_single() &amp;&amp; !is_attachment() ) {
            if ( get_post_type() != 'post' ) {
                $post_type = get_post_type_object(get_post_type());
                $slug = $post_type-&gt;rewrite;
                printf($link, $home_link . '/' . $slug['slug'] . '/', $post_type-&gt;labels-&gt;singular_name);
                if ($show_current == 1) echo $delimiter . $before . get_the_title() . $after;
            } else {
                $cat = get_the_category(); $cat = $cat[0];
                $cats = get_category_parents($cat, TRUE, $delimiter);
                if ($show_current == 0) $cats = preg_replace("#^(.+)$delimiter$#", "$1", $cats);
                $cats = str_replace('&lt;a', $link_before . '&lt;a' . $link_attr, $cats);
                $cats = str_replace('&lt;/a&gt;', '&lt;/a&gt;' . $link_after, $cats);
                if ($show_title == 0) $cats = preg_replace('/ title="(.*?)"/', '', $cats);
                echo $cats;
                if ($show_current == 1) echo $before . get_the_title() . $after;
            }

        } elseif ( !is_single() &amp;&amp; !is_page() &amp;&amp; get_post_type() != 'post' &amp;&amp; !is_404() ) {
            $post_type = get_post_type_object(get_post_type());
            echo $before . $post_type-&gt;labels-&gt;singular_name . $after;

        } elseif ( is_attachment() ) {
            $parent = get_post($parent_id);
            $cat = get_the_category($parent-&gt;ID); $cat = $cat[0];
            $cats = get_category_parents($cat, TRUE, $delimiter);
            $cats = str_replace('&lt;a', $link_before . '&lt;a' . $link_attr, $cats);
            $cats = str_replace('&lt;/a&gt;', '&lt;/a&gt;' . $link_after, $cats);
            if ($show_title == 0) $cats = preg_replace('/ title="(.*?)"/', '', $cats);
            echo $cats;
            printf($link, get_permalink($parent), $parent-&gt;post_title);
            if ($show_current == 1) echo $delimiter . $before . get_the_title() . $after;

        } elseif ( is_page() &amp;&amp; !$parent_id ) {
            if ($show_current == 1) echo $before . get_the_title() . $after;

        } elseif ( is_page() &amp;&amp; $parent_id ) {
            if ($parent_id != $frontpage_id) {
                $breadcrumbs = array();
                while ($parent_id) {
                    $page = get_page($parent_id);
                    if ($parent_id != $frontpage_id) {
                        $breadcrumbs[] = sprintf($link, get_permalink($page-&gt;ID), get_the_title($page-&gt;ID));
                    }
                    $parent_id = $page-&gt;post_parent;
                }
                $breadcrumbs = array_reverse($breadcrumbs);
                for ($i = 0; $i &lt; count($breadcrumbs); $i++) {
                    echo $breadcrumbs[$i];
                    if ($i != count($breadcrumbs)-1) echo $delimiter;
                }
            }
            if ($show_current == 1) {
                if ($show_home_link == 1 || ($parent_id_2 != 0 &amp;&amp; $parent_id_2 != $frontpage_id)) echo $delimiter;
                echo $before . get_the_title() . $after;
            }

        } elseif ( is_tag() ) {
            echo $before . sprintf($text['tag'], single_tag_title('', false)) . $after;

        } elseif ( is_author() ) {
            global $author;
            $userdata = get_userdata($author);
            echo $before . sprintf($text['author'], $userdata-&gt;display_name) . $after;

        } elseif ( is_404() ) {
            echo $before . $text['404'] . $after;
        }

        if ( get_query_var('paged') ) {
            if ( is_category() || is_day() || is_month() || is_year() || is_search() || is_tag() || is_author() ) echo ' (';
            echo __('Page') . ' ' . get_query_var('paged');
            if ( is_category() || is_day() || is_month() || is_year() || is_search() || is_tag() || is_author() ) echo ')';
        }

        echo '&lt;/div&gt;&lt;!-- .breadcrumbs --&gt;';

    }
} // end dimox_breadcrumbs()</pre>

&nbsp;

使用如下代码插入到你想显示的位置:

<pre class="lang:php decode:true ">&lt;?php if (function_exists('dimox_breadcrumbs')) dimox_breadcrumbs(); ?&gt;</pre>

&nbsp;

<div class="wp_syntax"></div>

<menu id="userscript-search-by-image" type="context"></menu><menu id="userscript-search-by-image" type="context"></menu>