split and merge



def region_split_merge(img, intensity_threshold):
    def recursive_divide_merge(sub):
        h, w = sub.shape
        if h<=2 or w<=2:
            return sub
        if np.max(sub) - np.min(sub) <= intensity_threshold:
            return np.full(sub.shape, 128, dtype=np.uint8)
        mid_h, mid_w = h//2, w//2
        top_left_part = recursive_divide_merge(sub[:mid_h, :mid_w])
        top_right_part = recursive_divide_merge(sub[:mid_h, mid_w:])
        bottom_left_part = recursive_divide_merge(sub[mid_h:, :mid_w])
        bottom_right_part = recursive_divide_merge(sub[mid_h:, mid_w:])
        return np.vstack(
            (np.hstack((top_left_part, top_right_part)),
            np.hstack((bottom_left_part, bottom_right_part))),
        )
    return recursive_divide_merge(img)
threshold = 25
merged_img = region_split_merge(img_array1, threshold)
display_2_images(img_array1, merged_img)
