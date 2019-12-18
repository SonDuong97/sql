1. Tìm tất cả các jobs (public) theo một category
SELECT * FROM jobs
WHERE (jobs.is_public = 1) 
	AND jobs.category_id = 1

2. Tìm 5 job (public) mới nhất theo thứ tự thời gian (kèm theo username của người tạo job đó)
SELECT jobs.*, users.`username` FROM jobs
INNER JOIN users ON jobs.`creator` = users.`id`
WHERE jobs.`is_public` = 1
ORDER BY jobs.`created` DESC
LIMIT 5

3. Tìm tất cả các job (public) mà title chứa một đoạn string cho trước
SELECT * FROM jobs
WHERE title LIKE '%sinh%'
		AND is_public = 1

4. Tìm tất cả các user có (username, password) bằng một cặp string cho trước
SELECT * FROM users
WHERE CONCAT(`username`, ', ', `password`) IN ('son-duong, son')

5. Tìm tất cả các job do một user đăng theo user id
SELECT * FROM jobs
WHERE jobs.`creator` = 1

6. Chuyển tất cả các job do một user đăng sang public
UPDATE jobs
SET `is_public` = 1
WHERE jobs.`creator` = 1

7. Kiểm tra một user có thể ứng tuyển một job nào đó không (job phải public và người ứng tuyển ko phải là creator)
SELECT * FROM users
WHERE `username` = 'son-duong'
	AND users.`id` NOT IN (
		SELECT jobs.`creator` FROM jobs
		WHERE title = 'IT'
	)

8. Đăng ký một user ứng tuyển một job nào đó
INSERT applications (job_id, user_id, message, created, modified)
SELECT jobs.id, users.id, 'apply son-duong', '2019-07-01', '2019-07-01'
FROM users, jobs
WHERE users.username = 'son-duong' AND jobs.title = 'IT'