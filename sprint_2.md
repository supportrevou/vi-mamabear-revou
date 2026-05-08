# 🔍 Sprint 2: Discovery & Data Integration

## Sprint Overview

**Duration:** 2 Weeks  
**Sprint Goal:** Transition from placeholders to real data and enable deep product browsing  
**Status:** Planning  
**Sprint Period:** Week 3-4

---

## 🎯 Sprint Objectives

1. Replace all placeholder data with real backend integration
2. Implement comprehensive product discovery features
3. Enable users to browse and search products effectively
4. Build robust category management system
5. Implement cloud-based media storage for product images
6. Create detailed product viewing experience

---

## 👥 User Stories

### Customer User Stories

**US-2.1: View Product Details**
- **As a** customer
- **I want to** view detailed information about a product
- **So that** I can make an informed purchase decision

**Acceptance Criteria:**
- Product detail page displays product name, price, description, and specifications
- Multiple product images shown in gallery with zoom functionality
- Stock availability clearly indicated
- Product variants (size, color) are selectable
- Price updates when variant is selected
- Related products displayed at bottom of page
- Page is mobile responsive

---

**US-2.2: Search for Products**
- **As a** customer
- **I want to** search for products by keyword
- **So that** I can quickly find what I'm looking for

**Acceptance Criteria:**
- Search bar is prominently displayed in navbar
- Search returns relevant results based on product name, description, and SKU
- Search suggestions appear as user types (autocomplete)
- Search results page displays matching products
- "No results" message shown when no products match
- Recent searches saved locally

---

**US-2.3: Filter Products**
- **As a** customer
- **I want to** filter products by category, price, and other attributes
- **So that** I can narrow down my search results

**Acceptance Criteria:**
- Filter sidebar displays on product listing page
- User can filter by category (with subcategories)
- User can filter by price range using slider
- User can filter by stock availability
- Active filters are clearly displayed with option to remove
- Result count updates when filters are applied
- Filters work on mobile (drawer/modal)

---

**US-2.4: Sort Products**
- **As a** customer
- **I want to** sort products by different criteria
- **So that** I can find products that best match my preferences

**Acceptance Criteria:**
- Sort dropdown available on product listing page
- User can sort by: Newest, Price (Low to High), Price (High to Low), Most Popular
- Products reorder immediately when sort option selected
- Current sort option is clearly indicated
- Sort preference persists during browsing session

---

**US-2.5: Browse by Category**
- **As a** customer
- **I want to** browse products by category
- **So that** I can explore products in areas I'm interested in

**Acceptance Criteria:**
- Category listing page displays all categories with images
- Category detail page shows products in that category
- Subcategories are accessible from parent category
- Category navigation menu (mega menu on desktop)
- Breadcrumb navigation shows current category path
- Product count displayed for each category

---

**US-2.6: Select Product Variants**
- **As a** customer
- **I want to** select product variants like size and color
- **So that** I can purchase the exact product I want

**Acceptance Criteria:**
- Variant options displayed clearly on product detail page
- Color variants shown with visual swatches
- Size variants shown as selectable buttons
- Out-of-stock variants are disabled/grayed out
- Price updates based on selected variant
- Stock availability shown for selected variant

---

### Admin User Stories

**US-2.7: Manage Categories**
- **As an** admin
- **I want to** create and organize product categories
- **So that** customers can easily browse products by type

**Acceptance Criteria:**
- Admin can create new categories with name, description, and image
- Admin can create subcategories (hierarchical structure)
- Admin can edit category information
- Admin can delete categories (with validation)
- Admin can reorder categories
- Categories can be activated/deactivated

---

**US-2.8: Upload Product Images**
- **As an** admin
- **I want to** upload multiple images for each product
- **So that** customers can see products from different angles

**Acceptance Criteria:**
- Admin can upload multiple images per product
- Images are stored in cloud storage (Cloudinary/S3)
- Admin can set featured/main image
- Admin can reorder images (drag and drop)
- Admin can delete images
- Image validation (type, size, dimensions)
- Thumbnails automatically generated

---

**US-2.9: Manage Product Variants**
- **As an** admin
- **I want to** add variants to products (size, color, etc.)
- **So that** I can offer different options to customers

**Acceptance Criteria:**
- Admin can add variant types (size, color, material, etc.)
- Admin can add variant values for each type
- Admin can set variant-specific pricing
- Admin can set variant-specific stock levels
- Admin can set variant-specific SKUs
- Variants can be activated/deactivated

---

## 📋 Sprint Backlog

### Front-End Development Tasks

#### 1. Product Detail Page (PDP)
- [ ] Create product detail page layout (`/products/[slug]`)
- [ ] Implement product image gallery
  - Main image display with zoom functionality
  - Thumbnail navigation
  - Image lightbox/modal for full-screen view
  - Support for multiple images (carousel)
- [ ] Build product information section
  - Product title and SKU
  - Price display (with original price if discounted)
  - Stock availability indicator
  - Product description (rich text support)
  - Product specifications table
- [ ] Implement product variant selector
  - Size selector (if applicable)
  - Color selector with visual swatches
  - Dynamic price update based on variant
  - Stock validation per variant
  - Disabled state for out-of-stock variants
- [ ] Add quantity selector with stock validation
- [ ] Create "Add to Cart" button with loading states
- [ ] Implement breadcrumb navigation
- [ ] Add related products section (based on category)
- [ ] Implement product reviews section (UI only for now)
- [ ] Add social sharing buttons
- [ ] Ensure mobile-responsive design

#### 2. Authentication Integration
- [ ] Integrate login API with frontend form
  - Handle successful login (store JWT token)
  - Display error messages for invalid credentials
  - Implement "Remember me" functionality
- [ ] Integrate registration API
  - Handle successful registration
  - Show validation errors
  - Redirect to email verification page
- [ ] Implement password reset flow
  - Request reset email
  - Reset password with token
  - Success/error handling
- [ ] Create authentication context/state management
  - Store user session data
  - Provide auth status across app
  - Handle token refresh
- [ ] Implement protected routes
  - Redirect unauthenticated users
  - Show loading state during auth check
- [ ] Add logout functionality
  - Clear tokens and user data
  - Redirect to homepage
- [ ] Update navbar to show user info when logged in
  - User avatar/initials
  - Dropdown menu (Profile, Orders, Logout)
- [ ] Implement persistent login (token refresh)

#### 3. Product Listing Integration
- [ ] Replace homepage mock data with API calls
  - Fetch featured products
  - Fetch products by category
  - Handle loading states
  - Handle empty states
- [ ] Create product listing page (`/products`)
  - Grid/list view toggle
  - Pagination controls
  - Products per page selector
- [ ] Implement product card component enhancements
  - "Add to Cart" quick action
  - "Quick View" modal
  - Wishlist button (UI only)
  - Stock badge (In Stock, Low Stock, Out of Stock)
  - Discount badge
- [ ] Add loading skeletons for better UX
- [ ] Implement error handling and retry mechanisms
- [ ] Add "Load More" or infinite scroll option

#### 4. Search & Filter UI
- [ ] Implement functional search bar
  - Search input with autocomplete suggestions
  - Search results page
  - Highlight search terms in results
  - Recent searches (stored locally)
  - Popular searches suggestions
- [ ] Create filter sidebar/panel
  - Category filter (hierarchical)
  - Price range slider
  - Brand filter (if applicable)
  - Rating filter (prepare for future)
  - Stock availability filter
  - Apply/Clear filters buttons
- [ ] Implement sorting dropdown
  - Newest first
  - Price: Low to High
  - Price: High to Low
  - Most Popular
  - Best Rating (prepare for future)
- [ ] Add active filters display with remove option
- [ ] Show result count
- [ ] Implement mobile-friendly filter drawer
- [ ] Add filter state to URL parameters (for sharing)

#### 5. Category Pages
- [ ] Create category listing page (`/categories`)
  - Display all categories with images
  - Category card with product count
- [ ] Create category detail page (`/categories/[slug]`)
  - Category banner/header
  - Category description
  - Products in category (with filters)
  - Subcategory navigation
- [ ] Implement category navigation menu
  - Mega menu for desktop
  - Collapsible menu for mobile
  - Category icons/images

---

### Back-End Development Tasks

#### 1. Search & Filtering Logic
- [ ] Implement full-text search functionality
  - Search across product name, description, SKU
  - Use PostgreSQL full-text search or implement Elasticsearch
  - Rank results by relevance
  - Handle typos and partial matches
- [ ] Build advanced filtering system
  - Filter by category (including subcategories)
  - Filter by price range
  - Filter by stock availability
  - Filter by multiple attributes simultaneously
  - Support for dynamic filters based on category
- [ ] Implement sorting logic
  - Sort by price (ascending/descending)
  - Sort by creation date (newest first)
  - Sort by popularity (based on sales/views)
  - Sort by rating (prepare for future)
- [ ] Create search API endpoints:
  - `GET /api/products/search?q={query}` - Search products
  - `GET /api/products/search/suggestions?q={query}` - Autocomplete
  - `GET /api/products/filter` - Advanced filtering with multiple params
- [ ] Optimize database queries for performance
  - Add database indexes on searchable fields
  - Implement query result caching
  - Use pagination to limit result sets
- [ ] Implement search analytics (track popular searches)

#### 2. Category Management System
- [ ] Design Category database schema
  - id, name, slug, description, imageUrl
  - parentId (for hierarchical categories)
  - sortOrder, isActive, createdAt, updatedAt
- [ ] Create Prisma migration for categories
- [ ] Implement Category CRUD API endpoints:
  - `GET /api/categories` - List all categories (with hierarchy)
  - `GET /api/categories/:id` - Get single category
  - `GET /api/categories/:id/products` - Get products in category
  - `POST /api/categories` - Create category (admin only)
  - `PUT /api/categories/:id` - Update category (admin only)
  - `DELETE /api/categories/:id` - Delete category (admin only)
- [ ] Implement category hierarchy logic
  - Get parent category
  - Get child categories
  - Get category breadcrumb path
  - Get all products in category and subcategories
- [ ] Add category validation
  - Prevent circular references
  - Validate slug uniqueness
  - Check for products before deletion
- [ ] Update Product schema to include categoryId
- [ ] Seed database with sample categories (5-10 categories)
- [ ] Update API documentation

#### 3. Media Storage Integration
- [ ] Choose and set up cloud storage service
  - Option A: Cloudinary (recommended for ease of use)
  - Option B: AWS S3 (more control, lower cost at scale)
- [ ] Configure storage service credentials
- [ ] Implement image upload functionality
  - Single image upload
  - Multiple image upload
  - Image validation (type, size, dimensions)
  - Generate thumbnails and optimized versions
- [ ] Create file upload API endpoints:
  - `POST /api/upload/image` - Upload single image
  - `POST /api/upload/images` - Upload multiple images
  - `DELETE /api/upload/image/:id` - Delete image
- [ ] Implement signed URL generation (for secure uploads)
- [ ] Add image metadata storage
  - Link images to products
  - Store alt text for accessibility
  - Track image dimensions and file size
- [ ] Implement image optimization
  - Automatic compression
  - Format conversion (WebP support)
  - Responsive image variants
- [ ] Update Product and Category schemas with image fields
- [ ] Migrate existing placeholder images to cloud storage
- [ ] Implement image CDN caching

#### 4. Product Variant System
- [ ] Enhance ProductVariant schema
  - id, productId, variantType (size, color, etc.)
  - variantValue, sku, priceAdjustment
  - stock, isActive, sortOrder
- [ ] Create VariantCombination schema (for products with multiple variant types)
  - id, productId, combination (JSON), sku, price, stock
- [ ] Implement Variant API endpoints:
  - `GET /api/products/:id/variants` - Get all variants for a product
  - `POST /api/products/:id/variants` - Add variant (admin)
  - `PUT /api/variants/:id` - Update variant (admin)
  - `DELETE /api/variants/:id` - Delete variant (admin)
- [ ] Update product endpoints to include variant data
- [ ] Implement variant stock management
- [ ] Add variant validation logic
- [ ] Seed database with sample product variants

#### 5. API Performance Optimization
- [ ] Implement response caching
  - Cache product listings
  - Cache category data
  - Cache search results
  - Set appropriate cache TTL
- [ ] Add database query optimization
  - Review and optimize slow queries
  - Add missing indexes
  - Implement eager loading for related data
- [ ] Implement API rate limiting
  - Per-user rate limits
  - Per-endpoint rate limits
  - Return appropriate headers
- [ ] Add request/response compression (Gzip)
- [ ] Implement pagination for all list endpoints
- [ ] Add API response time monitoring
- [ ] Create database backup strategy

---

## 🔗 Integration Tasks

### Front-End + Back-End Integration
- [ ] Integrate Product Detail Page with product API
- [ ] Connect search functionality to search API
- [ ] Integrate filter UI with filtering API
- [ ] Connect category pages to category API
- [ ] Implement image upload in admin panel (prepare for Sprint 4)
- [ ] Test authentication flow across all new pages
- [ ] Verify error handling for all API calls
- [ ] Test loading states and empty states
- [ ] Validate responsive design on all new pages

---

## 📊 Definition of Done

### For Each User Story:
- [ ] Code is written and follows project coding standards
- [ ] Code is reviewed by at least one team member
- [ ] Unit tests are written and passing (BE)
- [ ] Integration tests are passing
- [ ] API documentation is updated
- [ ] Feature is tested on multiple browsers
- [ ] Feature is tested on mobile devices
- [ ] Performance benchmarks are met
- [ ] No critical bugs or security vulnerabilities
- [ ] Code is merged to main/develop branch

### For Sprint 2 Completion:
- [ ] Product Detail Page is fully functional
- [ ] Users can search and filter products
- [ ] Category system is working properly
- [ ] All product images are stored in cloud storage
- [ ] Product variants are displayed correctly
- [ ] Authentication is integrated across all pages
- [ ] All placeholder data is replaced with real data
- [ ] API response times are within acceptable limits
- [ ] Sprint Review demo is prepared
- [ ] Documentation is updated

---

## 🎨 Design Guidelines

### Product Detail Page
- **Layout:** Two-column layout (images left, info right) on desktop
- **Images:** High-quality product photos, minimum 800x800px
- **Typography:** Clear hierarchy (title > price > description)
- **CTA:** Prominent "Add to Cart" button (primary color)
- **Spacing:** Generous whitespace for readability

### Search & Filter UI
- **Search Bar:** Prominent placement in navbar
- **Filters:** Sidebar on desktop, drawer on mobile
- **Active Filters:** Clearly visible with easy removal
- **Results:** Grid layout with consistent card sizes
- **Empty State:** Helpful message with suggestions

### Category Pages
- **Category Cards:** Image-focused with overlay text
- **Navigation:** Breadcrumbs for easy backtracking
- **Subcategories:** Clearly distinguished from products

---

## 🔐 Security Considerations

### Image Upload Security
- Validate file types (only allow images)
- Limit file size (max 5MB per image)
- Sanitize file names
- Scan for malware (if possible)
- Use signed URLs for uploads
- Implement rate limiting on upload endpoints

### Search Security
- Sanitize search queries to prevent SQL injection
- Implement rate limiting on search endpoint
- Validate and escape user input
- Prevent search query manipulation

### API Security
- Ensure all endpoints have proper authentication
- Validate all query parameters
- Implement CSRF protection
- Use parameterized queries
- Log suspicious activities

---


## 📚 Resources & References

### Documentation
- [Cloudinary Documentation](https://cloudinary.com/documentation)
- [AWS S3 Documentation](https://docs.aws.amazon.com/s3/)
- [PostgreSQL Full-Text Search](https://www.postgresql.org/docs/current/textsearch.html)
- [Next.js Image Optimization](https://nextjs.org/docs/basic-features/image-optimization)

### Tools
- **Image Optimization:** TinyPNG, ImageOptim
- **API Testing:** Postman, Insomnia
- **Performance Monitoring:** New Relic, DataDog (for future)

---

## 📝 Sprint Retrospective Template

### What Went Well?
- (To be filled at end of sprint)

### What Could Be Improved?
- (To be filled at end of sprint)

### Action Items for Next Sprint
- (To be filled at end of sprint)

### Key Learnings
- (To be filled at end of sprint)

---

## ✅ Sprint Review Checklist

- [ ] Demo product search functionality
- [ ] Show product filtering by category and price
- [ ] Demonstrate Product Detail Page with variants
- [ ] Show image upload to cloud storage
- [ ] Demo category navigation
- [ ] Show authentication integration
- [ ] Demonstrate mobile responsiveness
- [ ] Review API performance metrics
- [ ] Discuss any technical challenges
- [ ] Present Sprint 3 preview (shopping cart & checkout)

---


## 📖 Implementation Tutorials

### Tutorial: Implementing Image Upload with Cloudinary (US-2.8)

This tutorial will guide you through implementing image upload functionality using Cloudinary as the cloud storage provider.

> **Note:** Jika ada kendala dengan Cloudinary (misalnya: limit gratis tercapai, masalah regional, atau preferensi lain), Anda bisa explore alternatif service seperti:
> - **AWS S3** - Lebih fleksibel, cocok untuk scale besar
> - **Supabase Storage** - Open source, mudah diintegrasikan
> - **ImageKit** - Fokus pada image optimization
> - **Uploadcare** - User-friendly dengan fitur lengkap
> - **Backblaze B2** - Cost-effective alternative untuk S3
> 
> Konsep implementasi tetap sama: upload, store, dan retrieve image URLs.

---

#### Step 1: Set Up Cloudinary Account

1. Sign up for a free Cloudinary account at https://cloudinary.com
2. After signing in, go to Dashboard to find your credentials:
   - Cloud Name
   - API Key
   - API Secret
3. Add these credentials to your `.env` file:
   ```
   CLOUDINARY_CLOUD_NAME=your_cloud_name
   CLOUDINARY_API_KEY=your_api_key
   CLOUDINARY_API_SECRET=your_api_secret
   ```

---

#### Step 2: Install Required Packages

For Backend (Node.js/Express):
```bash
npm install cloudinary multer multer-storage-cloudinary
```

For Frontend (Next.js):
```bash
npm install react-dropzone
```

---

#### Step 3: Configure Cloudinary in Backend

Create `src/config/cloudinary.js`:
```javascript
const cloudinary = require('cloudinary').v2;

cloudinary.config({
  cloud_name: process.env.CLOUDINARY_CLOUD_NAME,
  api_key: process.env.CLOUDINARY_API_KEY,
  api_secret: process.env.CLOUDINARY_API_SECRET
});

module.exports = cloudinary;
```

---

#### Step 4: Create Upload Middleware

Create `src/middleware/upload.js`:
```javascript
const multer = require('multer');
const { CloudinaryStorage } = require('multer-storage-cloudinary');
const cloudinary = require('../config/cloudinary');

const storage = new CloudinaryStorage({
  cloudinary: cloudinary,
  params: {
    folder: 'mamabear/products',
    allowed_formats: ['jpg', 'jpeg', 'png', 'webp'],
    transformation: [{ width: 1000, height: 1000, crop: 'limit' }]
  }
});

const upload = multer({
  storage: storage,
  limits: {
    fileSize: 5 * 1024 * 1024 // 5MB limit
  },
  fileFilter: (req, file, cb) => {
    if (file.mimetype.startsWith('image/')) {
      cb(null, true);
    } else {
      cb(new Error('Only image files are allowed!'), false);
    }
  }
});

module.exports = upload;
```

---

#### Step 5: Create Image Upload API Endpoints

Create `src/routes/upload.routes.js`:
```javascript
const express = require('express');
const router = express.Router();
const upload = require('../middleware/upload');
const { authenticate, isAdmin } = require('../middleware/auth');
const cloudinary = require('../config/cloudinary');

// Upload single image
router.post('/image', authenticate, isAdmin, upload.single('image'), async (req, res) => {
  try {
    if (!req.file) {
      return res.status(400).json({ error: 'No image file provided' });
    }

    res.status(200).json({
      message: 'Image uploaded successfully',
      imageUrl: req.file.path,
      publicId: req.file.filename
    });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

// Upload multiple images
router.post('/images', authenticate, isAdmin, upload.array('images', 10), async (req, res) => {
  try {
    if (!req.files || req.files.length === 0) {
      return res.status(400).json({ error: 'No image files provided' });
    }

    const uploadedImages = req.files.map(file => ({
      imageUrl: file.path,
      publicId: file.filename
    }));

    res.status(200).json({
      message: 'Images uploaded successfully',
      images: uploadedImages
    });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

// Delete image
router.delete('/image/:publicId', authenticate, isAdmin, async (req, res) => {
  try {
    const { publicId } = req.params;
    
    // Delete from Cloudinary
    await cloudinary.uploader.destroy(publicId);

    res.status(200).json({ message: 'Image deleted successfully' });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

module.exports = router;
```

---

#### Step 6: Update Product Schema for Images

Update your Prisma schema to include product images:
```prisma
model Product {
  id          String   @id @default(uuid())
  name        String
  slug        String   @unique
  description String?
  price       Decimal
  stock       Int
  categoryId  String?
  category    Category? @relation(fields: [categoryId], references: [id])
  images      ProductImage[]
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
}

model ProductImage {
  id          String   @id @default(uuid())
  productId   String
  product     Product  @relation(fields: [productId], references: [id], onDelete: Cascade)
  imageUrl    String
  publicId    String
  altText     String?
  sortOrder   Int      @default(0)
  isFeatured  Boolean  @default(false)
  createdAt   DateTime @default(now())
}
```

Run migration:
```bash
npx prisma migrate dev --name add_product_images
```

---

#### Step 7: Create Product Image Management Endpoints

Add to `src/routes/products.routes.js`:
```javascript
// Add images to product
router.post('/:id/images', authenticate, isAdmin, async (req, res) => {
  try {
    const { id } = req.params;
    const { images } = req.body; // Array of { imageUrl, publicId, altText, isFeatured }

    const product = await prisma.product.findUnique({ where: { id } });
    if (!product) {
      return res.status(404).json({ error: 'Product not found' });
    }

    const createdImages = await prisma.productImage.createMany({
      data: images.map((img, index) => ({
        productId: id,
        imageUrl: img.imageUrl,
        publicId: img.publicId,
        altText: img.altText || product.name,
        sortOrder: index,
        isFeatured: img.isFeatured || false
      }))
    });

    res.status(201).json({
      message: 'Images added to product',
      count: createdImages.count
    });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

// Update image order
router.put('/:id/images/reorder', authenticate, isAdmin, async (req, res) => {
  try {
    const { id } = req.params;
    const { imageIds } = req.body; // Array of image IDs in new order

    await Promise.all(
      imageIds.map((imageId, index) =>
        prisma.productImage.update({
          where: { id: imageId },
          data: { sortOrder: index }
        })
      )
    );

    res.status(200).json({ message: 'Image order updated' });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

// Delete product image
router.delete('/:productId/images/:imageId', authenticate, isAdmin, async (req, res) => {
  try {
    const { productId, imageId } = req.params;

    const image = await prisma.productImage.findUnique({ where: { id: imageId } });
    if (!image) {
      return res.status(404).json({ error: 'Image not found' });
    }

    // Delete from Cloudinary
    await cloudinary.uploader.destroy(image.publicId);

    // Delete from database
    await prisma.productImage.delete({ where: { id: imageId } });

    res.status(200).json({ message: 'Image deleted successfully' });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});
```

---

#### Step 8: Create Frontend Image Upload Component

Create `components/admin/ImageUpload.jsx`:
```jsx
import { useState, useCallback } from 'react';
import { useDropzone } from 'react-dropzone';
import axios from 'axios';

export default function ImageUpload({ productId, onUploadComplete }) {
  const [uploading, setUploading] = useState(false);
  const [uploadedImages, setUploadedImages] = useState([]);

  const onDrop = useCallback(async (acceptedFiles) => {
    setUploading(true);

    try {
      // Upload images to server
      const formData = new FormData();
      acceptedFiles.forEach(file => {
        formData.append('images', file);
      });

      const uploadResponse = await axios.post('/api/upload/images', formData, {
        headers: {
          'Content-Type': 'multipart/form-data',
          'Authorization': `Bearer ${localStorage.getItem('token')}`
        }
      });

      const images = uploadResponse.data.images;

      // Associate images with product
      if (productId) {
        await axios.post(`/api/products/${productId}/images`, {
          images: images.map(img => ({
            imageUrl: img.imageUrl,
            publicId: img.publicId
          }))
        }, {
          headers: {
            'Authorization': `Bearer ${localStorage.getItem('token')}`
          }
        });
      }

      setUploadedImages([...uploadedImages, ...images]);
      onUploadComplete && onUploadComplete(images);
    } catch (error) {
      console.error('Upload error:', error);
      alert('Failed to upload images');
    } finally {
      setUploading(false);
    }
  }, [productId, uploadedImages, onUploadComplete]);

  const { getRootProps, getInputProps, isDragActive } = useDropzone({
    onDrop,
    accept: {
      'image/*': ['.jpeg', '.jpg', '.png', '.webp']
    },
    maxSize: 5242880, // 5MB
    multiple: true
  });

  const handleDelete = async (imageId, publicId) => {
    try {
      await axios.delete(`/api/upload/image/${publicId}`, {
        headers: {
          'Authorization': `Bearer ${localStorage.getItem('token')}`
        }
      });

      setUploadedImages(uploadedImages.filter(img => img.publicId !== publicId));
    } catch (error) {
      console.error('Delete error:', error);
      alert('Failed to delete image');
    }
  };

  return (
    <div className="space-y-4">
      <div
        {...getRootProps()}
        className={`border-2 border-dashed rounded-lg p-8 text-center cursor-pointer transition-colors ${
          isDragActive ? 'border-blue-500 bg-blue-50' : 'border-gray-300 hover:border-gray-400'
        }`}
      >
        <input {...getInputProps()} />
        {uploading ? (
          <p className="text-gray-600">Uploading...</p>
        ) : isDragActive ? (
          <p className="text-blue-600">Drop the images here...</p>
        ) : (
          <div>
            <p className="text-gray-600">Drag & drop images here, or click to select</p>
            <p className="text-sm text-gray-400 mt-2">Max 5MB per image, JPG/PNG/WEBP</p>
          </div>
        )}
      </div>

      {uploadedImages.length > 0 && (
        <div className="grid grid-cols-4 gap-4">
          {uploadedImages.map((image, index) => (
            <div key={index} className="relative group">
              <img
                src={image.imageUrl}
                alt={`Upload ${index + 1}`}
                className="w-full h-32 object-cover rounded-lg"
              />
              <button
                onClick={() => handleDelete(image.id, image.publicId)}
                className="absolute top-2 right-2 bg-red-500 text-white p-1 rounded-full opacity-0 group-hover:opacity-100 transition-opacity"
              >
                ✕
              </button>
            </div>
          ))}
        </div>
      )}
    </div>
  );
}
```

---

#### Step 9: Testing the Implementation

1. **Test Single Image Upload:**
   ```bash
   curl -X POST http://localhost:3000/api/upload/image \
     -H "Authorization: Bearer YOUR_TOKEN" \
     -F "image=@/path/to/image.jpg"
   ```

2. **Test Multiple Images Upload:**
   ```bash
   curl -X POST http://localhost:3000/api/upload/images \
     -H "Authorization: Bearer YOUR_TOKEN" \
     -F "images=@/path/to/image1.jpg" \
     -F "images=@/path/to/image2.jpg"
   ```

3. **Test Image Deletion:**
   ```bash
   curl -X DELETE http://localhost:3000/api/upload/image/PUBLIC_ID \
     -H "Authorization: Bearer YOUR_TOKEN"
   ```

---

#### Step 10: Best Practices & Optimization

1. **Image Optimization:**
   - Use Cloudinary transformations for automatic resizing
   - Generate thumbnails for faster loading
   - Use WebP format for better compression

2. **Security:**
   - Always validate file types on backend
   - Implement rate limiting on upload endpoints
   - Use signed uploads for sensitive operations

3. **User Experience:**
   - Show upload progress
   - Display image previews immediately
   - Allow drag-and-drop reordering
   - Provide clear error messages

4. **Performance:**
   - Lazy load images on product pages
   - Use Cloudinary's CDN for fast delivery
   - Implement image caching strategies

---
